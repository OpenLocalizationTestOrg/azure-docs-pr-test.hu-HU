---
title: "aaaAdd egy Azure Active Directory B2B együttműködés felhasználói tooa szerepkör |} Microsoft Docs"
description: "Vegye fel a Vendég felhasználói tooa szerepkört az Azure Active Directoryban"
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
ms.openlocfilehash: ccc58a0c8ecc73f8e79a8d827efdc0ff93846a96
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="grant-permissions-toousers-from-partner-organizations-in-your-azure-active-directory-tenant"></a><span data-ttu-id="e4261-103">Engedélyek toousers adja meg a fiókpartner-szervezetek, az Azure Active Directory-bérlőben</span><span class="sxs-lookup"><span data-stu-id="e4261-103">Grant permissions toousers from partner organizations in your Azure Active Directory tenant</span></span>

<span data-ttu-id="e4261-104">Az Azure Active Directory (Azure AD) B2B együttműködés felhasználók adódnak, Vendég felhasználó toohello könyvtár, és a Vendég engedélye hello könyvtárban korlátozott alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="e4261-104">Azure Active Directory (Azure AD) B2B collaboration users are added as guest users toohello directory, and guest permissions in hello directory are restricted by default.</span></span> <span data-ttu-id="e4261-105">A vállalat esetleg néhány vendég felhasználók toofill magasabb szintű szerepkört a szervezetében.</span><span class="sxs-lookup"><span data-stu-id="e4261-105">Your business may need some guest users toofill higher-privilege roles in your organization.</span></span> <span data-ttu-id="e4261-106">magasabb jogosultsági szerepkörök definiálása toosupport vendégfelhasználók lehet felügyelni, a szervezet igényei alapján hozzáadott tooany szerepkörök.</span><span class="sxs-lookup"><span data-stu-id="e4261-106">toosupport defining higher-privilege roles, guest users can be added tooany roles you desire, based on your organization's needs.</span></span>

## <a name="default-role"></a><span data-ttu-id="e4261-107">alapértelmezett szerepkör</span><span class="sxs-lookup"><span data-stu-id="e4261-107">Default role</span></span>

![alapértelmezett szerepkör](./media/active-directory-b2b-add-guest-to-role/default-role.png)

## <a name="global-administrator-role"></a><span data-ttu-id="e4261-109">Globális rendszergazdai szerepkörrel</span><span class="sxs-lookup"><span data-stu-id="e4261-109">Global Administrator Role</span></span>

![globális rendszergazdai szerepkörrel](./media/active-directory-b2b-add-guest-to-role/global-admin-role.png)

## <a name="limited-administrator-role"></a><span data-ttu-id="e4261-111">Korlátozott rendszergazdai szerepkör</span><span class="sxs-lookup"><span data-stu-id="e4261-111">Limited Administrator Role</span></span>

![korlátozott rendszergazdai szerepkörrel](./media/active-directory-b2b-add-guest-to-role/limited-admin-role.png)

## <a name="next-steps"></a><span data-ttu-id="e4261-113">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e4261-113">Next steps</span></span>

<span data-ttu-id="e4261-114">Ismerje meg az Azure AD B2B együttműködés további cikkeit:</span><span class="sxs-lookup"><span data-stu-id="e4261-114">Browse our other articles on Azure AD B2B collaboration:</span></span>

* [<span data-ttu-id="e4261-115">Mi az az Azure AD B2B együttműködés?</span><span class="sxs-lookup"><span data-stu-id="e4261-115">What is Azure AD B2B collaboration?</span></span>](active-directory-b2b-what-is-azure-ad-b2b.md)
* [<span data-ttu-id="e4261-116">B2B együttműködés felhasználó tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="e4261-116">B2B collaboration user properties</span></span>](active-directory-b2b-user-properties.md)
* [<span data-ttu-id="e4261-117">B2bB együttműködés meghívókat delegálása</span><span class="sxs-lookup"><span data-stu-id="e4261-117">Delegate B2bB collaboration invitations</span></span>](active-directory-b2b-delegate-invitations.md)
* [<span data-ttu-id="e4261-118">Dinamikus csoportok és a B2B együttműködés</span><span class="sxs-lookup"><span data-stu-id="e4261-118">Dynamic groups and B2B collaboration</span></span>](active-directory-b2b-dynamic-groups.md)
* [<span data-ttu-id="e4261-119">B2B együttműködés kód és a PowerShell-példák</span><span class="sxs-lookup"><span data-stu-id="e4261-119">B2B collaboration code and PowerShell samples</span></span>](active-directory-b2b-code-samples.md)
* [<span data-ttu-id="e4261-120">B2B együttműködés SaaS-alkalmazások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="e4261-120">Configure SaaS apps for B2B collaboration</span></span>](active-directory-b2b-configure-saas-apps.md)
* [<span data-ttu-id="e4261-121">B2B együttműködés felhasználói jogkivonatokhoz</span><span class="sxs-lookup"><span data-stu-id="e4261-121">B2B collaboration user tokens</span></span>](active-directory-b2b-user-token.md)
* [<span data-ttu-id="e4261-122">B2B együttműködés felhasználói jogcímek leképezése</span><span class="sxs-lookup"><span data-stu-id="e4261-122">B2B collaboration user claims mapping</span></span>](active-directory-b2b-claims-mapping.md)
* [<span data-ttu-id="e4261-123">Külső Office 365-megosztás</span><span class="sxs-lookup"><span data-stu-id="e4261-123">Office 365 external sharing</span></span>](active-directory-b2b-o365-external-user.md)
* [<span data-ttu-id="e4261-124">B2B együttműködés aktuális korlátozásai</span><span class="sxs-lookup"><span data-stu-id="e4261-124">B2B collaboration current limitations</span></span>](active-directory-b2b-current-limitations.md)
