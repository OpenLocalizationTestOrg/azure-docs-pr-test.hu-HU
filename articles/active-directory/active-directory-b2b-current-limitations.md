---
title: "az Azure Active Directory B2B együttműködés aaaLimitations |} Microsoft Docs"
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
ms.openlocfilehash: 322081f32fbacfe67ee1300993c7df1870e498bd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="limitations-of-azure-ad-b2b-collaboration"></a><span data-ttu-id="91e41-103">Azure AD B2B együttműködés korlátozásai</span><span class="sxs-lookup"><span data-stu-id="91e41-103">Limitations of Azure AD B2B collaboration</span></span>
<span data-ttu-id="91e41-104">Az Azure Active Directory (Azure AD) B2B együttműködés jelenleg ebben a cikkben ismertetett tulajdonos toohello korlátozások.</span><span class="sxs-lookup"><span data-stu-id="91e41-104">Azure Active Directory (Azure AD) B2B collaboration is currently subject toohello limitations described in this article.</span></span>

## <a name="possible-double-multi-factor-authentication"></a><span data-ttu-id="91e41-105">Lehetséges dupla többtényezős hitelesítés</span><span class="sxs-lookup"><span data-stu-id="91e41-105">Possible double multi-factor authentication</span></span>
<span data-ttu-id="91e41-106">Az Azure AD B2B kényszerítheti a többtényezős hitelesítés hello erőforrás-szervezetben (szervezet meghívása hello).</span><span class="sxs-lookup"><span data-stu-id="91e41-106">With Azure AD B2B, you can enforce multi-factor authentication at hello resource organization (hello inviting organization).</span></span> <span data-ttu-id="91e41-107">Ez a megközelítés hello okait részletezi [feltételes hozzáférés a B2B együttműködés felhasználók](active-directory-b2b-mfa-instructions.md).</span><span class="sxs-lookup"><span data-stu-id="91e41-107">hello reasons for this approach are detailed in [Conditional access for B2B collaboration users](active-directory-b2b-mfa-instructions.md).</span></span> <span data-ttu-id="91e41-108">Ha egy partnert már többtényezős hitelesítés beállítása, és azt, a felhasználóknak problémájuk lehet tooperform hello hitelesítési egyszer a otthoni szervezetek, majd ismét az Öné.</span><span class="sxs-lookup"><span data-stu-id="91e41-108">If a partner already has multi-factor authentication set up and enforced, their users might have tooperform hello authentication once in their home organization and then again in yours.</span></span>

## <a name="instant-on"></a><span data-ttu-id="91e41-109">Azonnali</span><span class="sxs-lookup"><span data-stu-id="91e41-109">Instant-on</span></span>
<span data-ttu-id="91e41-110">Hello B2B együttműködés adatfolyamok azt felhasználók toohello könyvtár hozzáadása, és dinamikusan frissítse azokat a meghívó érvényesítési és alkalmazás-hozzárendelés, stb.</span><span class="sxs-lookup"><span data-stu-id="91e41-110">In hello B2B collaboration flows, we add users toohello directory and dynamically update them during invitation redemption, app assignment, and so on.</span></span> <span data-ttu-id="91e41-111">hello frissítések és az írás szokásos fordul elő egy címtárpéldány, és minden példányára kell replikálni.</span><span class="sxs-lookup"><span data-stu-id="91e41-111">hello updates and writes ordinarily happen in one directory instance and must be replicated across all instances.</span></span> <span data-ttu-id="91e41-112">Replikáció befejezése után minden példány frissítése.</span><span class="sxs-lookup"><span data-stu-id="91e41-112">Replication is completed once all instances are updated.</span></span> <span data-ttu-id="91e41-113">Néha amikor hello objektum írt, vagy egy példány frissítése és a hello hívás tooretrieve Ez az objektum nem tooanother példány replikációs késések fordulnak elő akkor fordulhat elő.</span><span class="sxs-lookup"><span data-stu-id="91e41-113">Sometimes when hello object is written or updated in one instance and hello call tooretrieve this object is tooanother instance, replication latencies can occur.</span></span> <span data-ttu-id="91e41-114">Ebben az esetben, ha frissítse, vagy próbálkozzon újra a toohelp.</span><span class="sxs-lookup"><span data-stu-id="91e41-114">If that happens, refresh or retry toohelp.</span></span> <span data-ttu-id="91e41-115">Ha az alkalmazást az API, majd az ismételt próbálkozás egy biztonsági ki van egy megfelelő, a védelem gyakorlat tooalleviate probléma.</span><span class="sxs-lookup"><span data-stu-id="91e41-115">If you are writing an app using our API, then retries with some back-off is a good, defensive practice tooalleviate this issue.</span></span>

## <a name="next-steps"></a><span data-ttu-id="91e41-116">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="91e41-116">Next steps</span></span>

<span data-ttu-id="91e41-117">Ismerje meg az Azure AD B2B együttműködés további cikkeit:</span><span class="sxs-lookup"><span data-stu-id="91e41-117">Browse our other articles on Azure AD B2B collaboration:</span></span>

* [<span data-ttu-id="91e41-118">Mi az az Azure AD B2B együttműködés?</span><span class="sxs-lookup"><span data-stu-id="91e41-118">What is Azure AD B2B collaboration?</span></span>](active-directory-b2b-what-is-azure-ad-b2b.md)
* [<span data-ttu-id="91e41-119">B2B együttműködés felhasználó tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="91e41-119">B2B collaboration user properties</span></span>](active-directory-b2b-user-properties.md)
* [<span data-ttu-id="91e41-120">B2B együttműködés felhasználói tooa szerepkör hozzáadása</span><span class="sxs-lookup"><span data-stu-id="91e41-120">Adding a B2B collaboration user tooa role</span></span>](active-directory-b2b-add-guest-to-role.md)
* [<span data-ttu-id="91e41-121">B2bB együttműködés meghívókat delegálása</span><span class="sxs-lookup"><span data-stu-id="91e41-121">Delegate B2bB collaboration invitations</span></span>](active-directory-b2b-delegate-invitations.md)
* [<span data-ttu-id="91e41-122">Dinamikus csoportok és a B2B együttműködés</span><span class="sxs-lookup"><span data-stu-id="91e41-122">Dynamic groups and B2B collaboration</span></span>](active-directory-b2b-dynamic-groups.md)
* [<span data-ttu-id="91e41-123">B2B együttműködés kód és a PowerShell-példák</span><span class="sxs-lookup"><span data-stu-id="91e41-123">B2B collaboration code and PowerShell samples</span></span>](active-directory-b2b-code-samples.md)
* [<span data-ttu-id="91e41-124">B2B együttműködés SaaS-alkalmazások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="91e41-124">Configure SaaS apps for B2B collaboration</span></span>](active-directory-b2b-configure-saas-apps.md)
* [<span data-ttu-id="91e41-125">B2B együttműködés felhasználói jogkivonatokhoz</span><span class="sxs-lookup"><span data-stu-id="91e41-125">B2B collaboration user tokens</span></span>](active-directory-b2b-user-token.md)
* [<span data-ttu-id="91e41-126">B2B együttműködés felhasználói jogcímek leképezése</span><span class="sxs-lookup"><span data-stu-id="91e41-126">B2B collaboration user claims mapping</span></span>](active-directory-b2b-claims-mapping.md)
* [<span data-ttu-id="91e41-127">Külső Office 365-megosztás</span><span class="sxs-lookup"><span data-stu-id="91e41-127">Office 365 external sharing</span></span>](active-directory-b2b-o365-external-user.md)
