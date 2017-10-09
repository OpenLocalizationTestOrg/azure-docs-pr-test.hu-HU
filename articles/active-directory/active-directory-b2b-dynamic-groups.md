---
title: "aaaDynamic csoportok és az Azure Active Directory B2B együttműködés |} Microsoft Docs"
description: "Az Azure Active Directory B2B együttműködés használható az Azure AD dinamikus csoportok"
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
ms.date: 06/27/2017
ms.author: curtand
ms.reviewer: sasubram
ms.openlocfilehash: b011298de5fd2c851c6d9caaf5c2b257807ef0a4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="dynamic-groups-and-azure-active-directory-b2b-collaboration"></a><span data-ttu-id="43b04-103">Dinamikus csoportok és az Azure Active Directory B2B együttműködés</span><span class="sxs-lookup"><span data-stu-id="43b04-103">Dynamic groups and Azure Active Directory B2B collaboration</span></span>

## <a name="what-are-dynamic-groups"></a><span data-ttu-id="43b04-104">Mik azok a dinamikus csoportok?</span><span class="sxs-lookup"><span data-stu-id="43b04-104">What are dynamic groups?</span></span>
<span data-ttu-id="43b04-105">Az Azure Active Directory (Azure AD), biztonsági csoporttagság dinamikus beállítási lehetőségek érhetők el a [hello Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="43b04-105">Dynamic configuration of security group membership for Azure Active Directory (Azure AD) is available in [hello Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="43b04-106">A rendszergazdák az Azure Active Directory címtárban létrehozott toopopulate csoportok alapján, felhasználói attribútumok (például userType, részleg vagy ország) állíthatja be.</span><span class="sxs-lookup"><span data-stu-id="43b04-106">Administrators can set rules toopopulate groups that are created in Azure Active Directory based on user attributes (such as userType, department, or country).</span></span> <span data-ttu-id="43b04-107">Tagok automatikusan hozzáadhatók tooor attribútumaik alapján egy biztonsági csoportot eltávolítani.</span><span class="sxs-lookup"><span data-stu-id="43b04-107">Members can be automatically added tooor removed from a security group based on their attributes.</span></span> <span data-ttu-id="43b04-108">Ezek a csoportok hozzáférést biztosíthat tooapplications vagy a felhőbeli erőforrások (SharePoint-webhelyekhez, dokumentumok), és tooassign licencek toomembers.</span><span class="sxs-lookup"><span data-stu-id="43b04-108">These groups can provide access tooapplications or cloud resources (SharePoint sites, documents) and tooassign licenses toomembers.</span></span> <span data-ttu-id="43b04-109">További információk a dinamikus csoportok [dedikált csoportok az Azure Active Directoryban](active-directory-accessmanagement-dedicated-groups.md).</span><span class="sxs-lookup"><span data-stu-id="43b04-109">Read more about dynamic groups in [Dedicated groups in Azure Active Directory](active-directory-accessmanagement-dedicated-groups.md).</span></span>

<span data-ttu-id="43b04-110">megfelelő hello [Azure AD Premium P1 és P2 licencelési](https://azure.microsoft.com/pricing/details/active-directory/) szükséges toocreate és -felhasználási dinamikus csoportok van.</span><span class="sxs-lookup"><span data-stu-id="43b04-110">hello appropriate [Azure AD Premium P1 or P2 licensing](https://azure.microsoft.com/pricing/details/active-directory/) is required toocreate and use dynamic groups.</span></span> <span data-ttu-id="43b04-111">További információ: hello cikk [dinamikus csoporttagság Attribútumalapú szabályok létrehozása az Azure Active Directoryban](active-directory-groups-dynamic-membership-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="43b04-111">Learn more in hello article [Create attribute-based rules for dynamic group membership in Azure Active Directory](active-directory-groups-dynamic-membership-azure-portal.md).</span></span>

## <a name="what-are-hello-built-in-dynamic-groups"></a><span data-ttu-id="43b04-112">Mik azok a hello beépített dinamikus csoportot?</span><span class="sxs-lookup"><span data-stu-id="43b04-112">What are hello built-in dynamic groups?</span></span>
<span data-ttu-id="43b04-113">Hello **minden felhasználó** dinamikus csoport lehetővé teszi, hogy a bérlői rendszergazdák toocreate kattintson egy hello bérlő egyetlen összes felhasználót tartalmazó csoport.</span><span class="sxs-lookup"><span data-stu-id="43b04-113">hello **All users** dynamic group enables tenant admins toocreate a group containing all users in hello tenant with a single click.</span></span> <span data-ttu-id="43b04-114">Alapértelmezés szerint hello **minden felhasználó** csoport hello könyvtárban, beleértve a tagok és a vendégek felhasználókat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="43b04-114">By default, hello **All users** group includes all users in hello directory, including Members and Guests.</span></span>
<span data-ttu-id="43b04-115">Hello új Azure Active Directory felügyeleti portálon választhat tooenable hello **minden felhasználó** csoporthoz hello beállítások megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="43b04-115">Within hello new Azure Active Directory admin portal, you can choose tooenable hello **All users** group in hello Group Settings view.</span></span>

![beépített csoportok](media/active-directory-b2b-dynamic-groups/built-in-groups.png)

## <a name="hardening-hello-all-users-dynamic-group"></a><span data-ttu-id="43b04-117">Korlátozási hello minden felhasználók dinamikus csoport</span><span class="sxs-lookup"><span data-stu-id="43b04-117">Hardening hello All users dynamic group</span></span>
<span data-ttu-id="43b04-118">Alapértelmezés szerint hello **minden felhasználó** csoport tartalmazza a B2B együttműködés (vendég) felhasználókat is.</span><span class="sxs-lookup"><span data-stu-id="43b04-118">By default, hello **All users** group contains your B2B collaboration (guest) users as well.</span></span> <span data-ttu-id="43b04-119">További biztonságossá teheti a **minden felhasználó** használatával egy szabály tooremove vendég felhasználók csoportba.</span><span class="sxs-lookup"><span data-stu-id="43b04-119">You can further secure your **All users** group by using a rule tooremove guest users.</span></span> <span data-ttu-id="43b04-120">hello alábbi ábrán látható hello **minden felhasználó** csoport tooexclude vendégek módosítva.</span><span class="sxs-lookup"><span data-stu-id="43b04-120">hello following illustration shows hello **All users** group modified tooexclude guests.</span></span>

![minden felhasználói csoport engedélyezése](media/active-directory-b2b-dynamic-groups/enable-all-users-group.png)

<span data-ttu-id="43b04-122">Is találhat, hasznos toocreate tartalmazó új dinamikus csoportok csak a vendégfelhasználók számára, hogy a házirendek (például az Azure AD feltételes hozzáférési házirendek) toothem is alkalmazhat.</span><span class="sxs-lookup"><span data-stu-id="43b04-122">You might also find it useful toocreate a new dynamic group that contains only guest users, so that you can apply policies (such as Azure AD Conditional Access policies) toothem.</span></span>
<span data-ttu-id="43b04-123">Milyen ilyen csoporthoz nézhet ki:</span><span class="sxs-lookup"><span data-stu-id="43b04-123">What such a group might look like:</span></span>

![vendégfelhasználók kizárása](media/active-directory-b2b-dynamic-groups/exclude-guest-users.png)

## <a name="next-steps"></a><span data-ttu-id="43b04-125">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="43b04-125">Next steps</span></span>

<span data-ttu-id="43b04-126">Ismerje meg az Azure AD B2B együttműködés további cikkeit:</span><span class="sxs-lookup"><span data-stu-id="43b04-126">Browse our other articles on Azure AD B2B collaboration:</span></span>

* [<span data-ttu-id="43b04-127">Mi az az Azure AD B2B együttműködés?</span><span class="sxs-lookup"><span data-stu-id="43b04-127">What is Azure AD B2B collaboration?</span></span>](active-directory-b2b-what-is-azure-ad-b2b.md)
* [<span data-ttu-id="43b04-128">B2B együttműködés felhasználó tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="43b04-128">B2B collaboration user properties</span></span>](active-directory-b2b-user-properties.md)
* [<span data-ttu-id="43b04-129">B2B együttműködés felhasználói tooa szerepkör hozzáadása</span><span class="sxs-lookup"><span data-stu-id="43b04-129">Adding a B2B collaboration user tooa role</span></span>](active-directory-b2b-add-guest-to-role.md)
* [<span data-ttu-id="43b04-130">B2B együttműködés meghívókat delegálása</span><span class="sxs-lookup"><span data-stu-id="43b04-130">Delegate B2B collaboration invitations</span></span>](active-directory-b2b-delegate-invitations.md)
* [<span data-ttu-id="43b04-131">B2B együttműködés kód és a PowerShell-példák</span><span class="sxs-lookup"><span data-stu-id="43b04-131">B2B collaboration code and PowerShell samples</span></span>](active-directory-b2b-code-samples.md)
* [<span data-ttu-id="43b04-132">B2B együttműködés SaaS-alkalmazások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="43b04-132">Configure SaaS apps for B2B collaboration</span></span>](active-directory-b2b-configure-saas-apps.md)
* [<span data-ttu-id="43b04-133">B2B együttműködés felhasználói jogkivonatokhoz</span><span class="sxs-lookup"><span data-stu-id="43b04-133">B2B collaboration user tokens</span></span>](active-directory-b2b-user-token.md)
* [<span data-ttu-id="43b04-134">B2B együttműködés felhasználói jogcímek leképezése</span><span class="sxs-lookup"><span data-stu-id="43b04-134">B2B collaboration user claims mapping</span></span>](active-directory-b2b-claims-mapping.md)
* [<span data-ttu-id="43b04-135">Külső Office 365-megosztás</span><span class="sxs-lookup"><span data-stu-id="43b04-135">Office 365 external sharing</span></span>](active-directory-b2b-o365-external-user.md)
* [<span data-ttu-id="43b04-136">B2B együttműködés aktuális korlátozásai</span><span class="sxs-lookup"><span data-stu-id="43b04-136">B2B collaboration current limitations</span></span>](active-directory-b2b-current-limitations.md)
