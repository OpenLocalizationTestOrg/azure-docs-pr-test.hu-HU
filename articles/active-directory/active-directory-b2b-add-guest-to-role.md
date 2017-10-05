---
title: "Adja hozzá az Azure Active Directory B2B együttműködés felhasználó egy szerepkörhöz |} Microsoft Docs"
description: "A Vendég felhasználó hozzáadása egy szerepkört az Azure Active Directoryban"
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
ms.date: 03/15/2017
ms.author: sasubram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e816349ea971c997f655b4d51672dba666bc3e89
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="grant-permissions-to-users-from-partner-organizations-in-your-azure-active-directory-tenant"></a><span data-ttu-id="e71d1-103">Felhasználói engedélyek kiosztása a fiókpartner-szervezetek, az Azure Active Directory-bérlőben</span><span class="sxs-lookup"><span data-stu-id="e71d1-103">Grant permissions to users from partner organizations in your Azure Active Directory tenant</span></span>

<span data-ttu-id="e71d1-104">Az Azure Active Directory (Azure AD) B2B együttműködés felhasználók hozzá szeretné adni vendégfelhasználók számára a könyvtárba, és a címtárban Vendég engedélye korlátozott alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="e71d1-104">Azure Active Directory (Azure AD) B2B collaboration users are added as guest users to the directory, and guest permissions in the directory are restricted by default.</span></span> <span data-ttu-id="e71d1-105">A vállalat esetleg néhány vendégfelhasználók a szervezet magasabb szintű szerepkörök betöltésére.</span><span class="sxs-lookup"><span data-stu-id="e71d1-105">Your business may need some guest users to fill higher-privilege roles in your organization.</span></span> <span data-ttu-id="e71d1-106">Magasabb jogosultsági szerepkörök definiálása támogatásához vendégfelhasználók egyetlen szerepkörhöz sem felügyelni, a szervezet igényei alapján is hozzáadhatók.</span><span class="sxs-lookup"><span data-stu-id="e71d1-106">To support defining higher-privilege roles, guest users can be added to any roles you desire, based on your organization's needs.</span></span>

## <a name="default-role"></a><span data-ttu-id="e71d1-107">alapértelmezett szerepkör</span><span class="sxs-lookup"><span data-stu-id="e71d1-107">Default role</span></span>

![alapértelmezett szerepkör](./media/active-directory-b2b-add-guest-to-role/default-role.png)

## <a name="global-administrator-role"></a><span data-ttu-id="e71d1-109">Globális rendszergazdai szerepkörrel</span><span class="sxs-lookup"><span data-stu-id="e71d1-109">Global Administrator Role</span></span>

![globális rendszergazdai szerepkörrel](./media/active-directory-b2b-add-guest-to-role/global-admin-role.png)

## <a name="limited-administrator-role"></a><span data-ttu-id="e71d1-111">Korlátozott rendszergazdai szerepkör</span><span class="sxs-lookup"><span data-stu-id="e71d1-111">Limited Administrator Role</span></span>

![korlátozott rendszergazdai szerepkörrel](./media/active-directory-b2b-add-guest-to-role/limited-admin-role.png)

## <a name="next-steps"></a><span data-ttu-id="e71d1-113">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e71d1-113">Next steps</span></span>

<span data-ttu-id="e71d1-114">Ismerje meg az Azure AD B2B együttműködés további cikkeit:</span><span class="sxs-lookup"><span data-stu-id="e71d1-114">Browse our other articles on Azure AD B2B collaboration:</span></span>

* [<span data-ttu-id="e71d1-115">Mi az az Azure AD B2B együttműködés?</span><span class="sxs-lookup"><span data-stu-id="e71d1-115">What is Azure AD B2B collaboration?</span></span>](active-directory-b2b-what-is-azure-ad-b2b.md)
* [<span data-ttu-id="e71d1-116">B2B együttműködés felhasználó tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="e71d1-116">B2B collaboration user properties</span></span>](active-directory-b2b-user-properties.md)
* [<span data-ttu-id="e71d1-117">B2bB együttműködés meghívókat delegálása</span><span class="sxs-lookup"><span data-stu-id="e71d1-117">Delegate B2bB collaboration invitations</span></span>](active-directory-b2b-delegate-invitations.md)
* [<span data-ttu-id="e71d1-118">Dinamikus csoportok és a B2B együttműködés</span><span class="sxs-lookup"><span data-stu-id="e71d1-118">Dynamic groups and B2B collaboration</span></span>](active-directory-b2b-dynamic-groups.md)
* [<span data-ttu-id="e71d1-119">B2B együttműködés kód és a PowerShell-példák</span><span class="sxs-lookup"><span data-stu-id="e71d1-119">B2B collaboration code and PowerShell samples</span></span>](active-directory-b2b-code-samples.md)
* [<span data-ttu-id="e71d1-120">B2B együttműködés SaaS-alkalmazások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="e71d1-120">Configure SaaS apps for B2B collaboration</span></span>](active-directory-b2b-configure-saas-apps.md)
* [<span data-ttu-id="e71d1-121">B2B együttműködés felhasználói jogkivonatokhoz</span><span class="sxs-lookup"><span data-stu-id="e71d1-121">B2B collaboration user tokens</span></span>](active-directory-b2b-user-token.md)
* [<span data-ttu-id="e71d1-122">B2B együttműködés felhasználói jogcímek leképezése</span><span class="sxs-lookup"><span data-stu-id="e71d1-122">B2B collaboration user claims mapping</span></span>](active-directory-b2b-claims-mapping.md)
* [<span data-ttu-id="e71d1-123">Külső Office 365-megosztás</span><span class="sxs-lookup"><span data-stu-id="e71d1-123">Office 365 external sharing</span></span>](active-directory-b2b-o365-external-user.md)
* [<span data-ttu-id="e71d1-124">B2B együttműködés aktuális korlátozásai</span><span class="sxs-lookup"><span data-stu-id="e71d1-124">B2B collaboration current limitations</span></span>](active-directory-b2b-current-limitations.md)
