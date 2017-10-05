---
title: "Azure Active Directory B2B együttműködés korlátozásai |} Microsoft Docs"
description: "Azure Active Directory B2B együttműködés aktuális korlátozásai"
services: active-directory
documentationcenter: 
author: sasubram
manager: femila
editor: 
tags: 
ms.assetid: 
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 05/23/2017
ms.author: sasubram
ms.openlocfilehash: 581e5d1fb5fb08d0dc89ed2c85edcb5f0005650b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="limitations-of-azure-ad-b2b-collaboration"></a><span data-ttu-id="ae9d5-103">Azure AD B2B együttműködés korlátozásai</span><span class="sxs-lookup"><span data-stu-id="ae9d5-103">Limitations of Azure AD B2B collaboration</span></span>
<span data-ttu-id="ae9d5-104">Az Azure Active Directory (Azure AD) B2B együttműködés jelenleg a jelen cikkben ismertetett korlátozások vonatkoznak.</span><span class="sxs-lookup"><span data-stu-id="ae9d5-104">Azure Active Directory (Azure AD) B2B collaboration is currently subject to the limitations described in this article.</span></span>

## <a name="possible-double-multi-factor-authentication"></a><span data-ttu-id="ae9d5-105">Lehetséges dupla többtényezős hitelesítés</span><span class="sxs-lookup"><span data-stu-id="ae9d5-105">Possible double multi-factor authentication</span></span>
<span data-ttu-id="ae9d5-106">Az Azure AD B2B kényszerítheti a többtényezős hitelesítést, az erőforrás-szervezet (a hívja fel szervezet).</span><span class="sxs-lookup"><span data-stu-id="ae9d5-106">With Azure AD B2B, you can enforce multi-factor authentication at the resource organization (the inviting organization).</span></span> <span data-ttu-id="ae9d5-107">Ez a megközelítés okait részletezi [feltételes hozzáférés a B2B együttműködés felhasználók](active-directory-b2b-mfa-instructions.md).</span><span class="sxs-lookup"><span data-stu-id="ae9d5-107">The reasons for this approach are detailed in [Conditional access for B2B collaboration users](active-directory-b2b-mfa-instructions.md).</span></span> <span data-ttu-id="ae9d5-108">Ha egy partnert már többtényezős hitelesítés beállítása, és azt, a felhasználók esetleg el kell végeznie a hitelesítési egyszer a otthoni szervezetek, majd ismét az Öné.</span><span class="sxs-lookup"><span data-stu-id="ae9d5-108">If a partner already has multi-factor authentication set up and enforced, their users might have to perform the authentication once in their home organization and then again in yours.</span></span>

## <a name="instant-on"></a><span data-ttu-id="ae9d5-109">Azonnali</span><span class="sxs-lookup"><span data-stu-id="ae9d5-109">Instant-on</span></span>
<span data-ttu-id="ae9d5-110">A B2B együttműködés adatfolyamok azt felhasználók hozzáadása a címtárhoz és dinamikusan frissítse azokat a meghívó érvényesítési és alkalmazás-hozzárendelés, stb.</span><span class="sxs-lookup"><span data-stu-id="ae9d5-110">In the B2B collaboration flows, we add users to the directory and dynamically update them during invitation redemption, app assignment, and so on.</span></span> <span data-ttu-id="ae9d5-111">A frissítések és az írás szokásos fordul elő egy címtárpéldány, és minden példányára kell replikálni.</span><span class="sxs-lookup"><span data-stu-id="ae9d5-111">The updates and writes ordinarily happen in one directory instance and must be replicated across all instances.</span></span> <span data-ttu-id="ae9d5-112">Replikáció befejezése után minden példány frissítése.</span><span class="sxs-lookup"><span data-stu-id="ae9d5-112">Replication is completed once all instances are updated.</span></span> <span data-ttu-id="ae9d5-113">Egyes esetekben, amikor a objektumot írt vagy egy példány frissítése, és ez az objektum beolvasása a hívást, hogy egy másik példánya, a replikációs késések fordulnak elő akkor fordulhat elő.</span><span class="sxs-lookup"><span data-stu-id="ae9d5-113">Sometimes when the object is written or updated in one instance and the call to retrieve this object is to another instance, replication latencies can occur.</span></span> <span data-ttu-id="ae9d5-114">Ha ez előfordul, frissítse vagy segítségével próbálja meg újra.</span><span class="sxs-lookup"><span data-stu-id="ae9d5-114">If that happens, refresh or retry to help.</span></span> <span data-ttu-id="ae9d5-115">Ha az alkalmazást az API felületen, néhány biztonsági off újrapróbálkozások egy megfelelő, a védelem gyakorlat, hogy enyhítse a probléma.</span><span class="sxs-lookup"><span data-stu-id="ae9d5-115">If you are writing an app using our API, then retries with some back-off is a good, defensive practice to alleviate this issue.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ae9d5-116">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ae9d5-116">Next steps</span></span>

<span data-ttu-id="ae9d5-117">Ismerje meg az Azure AD B2B együttműködés további cikkeit:</span><span class="sxs-lookup"><span data-stu-id="ae9d5-117">Browse our other articles on Azure AD B2B collaboration:</span></span>

* [<span data-ttu-id="ae9d5-118">Mi az az Azure AD B2B együttműködés?</span><span class="sxs-lookup"><span data-stu-id="ae9d5-118">What is Azure AD B2B collaboration?</span></span>](active-directory-b2b-what-is-azure-ad-b2b.md)
* [<span data-ttu-id="ae9d5-119">B2B együttműködés felhasználó tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="ae9d5-119">B2B collaboration user properties</span></span>](active-directory-b2b-user-properties.md)
* [<span data-ttu-id="ae9d5-120">Egy szerepkör B2B együttműködés felhasználók hozzáadása</span><span class="sxs-lookup"><span data-stu-id="ae9d5-120">Adding a B2B collaboration user to a role</span></span>](active-directory-b2b-add-guest-to-role.md)
* [<span data-ttu-id="ae9d5-121">B2bB együttműködés meghívókat delegálása</span><span class="sxs-lookup"><span data-stu-id="ae9d5-121">Delegate B2bB collaboration invitations</span></span>](active-directory-b2b-delegate-invitations.md)
* [<span data-ttu-id="ae9d5-122">Dinamikus csoportok és a B2B együttműködés</span><span class="sxs-lookup"><span data-stu-id="ae9d5-122">Dynamic groups and B2B collaboration</span></span>](active-directory-b2b-dynamic-groups.md)
* [<span data-ttu-id="ae9d5-123">B2B együttműködés kód és a PowerShell-példák</span><span class="sxs-lookup"><span data-stu-id="ae9d5-123">B2B collaboration code and PowerShell samples</span></span>](active-directory-b2b-code-samples.md)
* [<span data-ttu-id="ae9d5-124">B2B együttműködés SaaS-alkalmazások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="ae9d5-124">Configure SaaS apps for B2B collaboration</span></span>](active-directory-b2b-configure-saas-apps.md)
* [<span data-ttu-id="ae9d5-125">B2B együttműködés felhasználói jogkivonatokhoz</span><span class="sxs-lookup"><span data-stu-id="ae9d5-125">B2B collaboration user tokens</span></span>](active-directory-b2b-user-token.md)
* [<span data-ttu-id="ae9d5-126">B2B együttműködés felhasználói jogcímek leképezése</span><span class="sxs-lookup"><span data-stu-id="ae9d5-126">B2B collaboration user claims mapping</span></span>](active-directory-b2b-claims-mapping.md)
* [<span data-ttu-id="ae9d5-127">Külső Office 365-megosztás</span><span class="sxs-lookup"><span data-stu-id="ae9d5-127">Office 365 external sharing</span></span>](active-directory-b2b-o365-external-user.md)
