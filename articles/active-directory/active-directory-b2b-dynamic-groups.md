---
title: "Dinamikus csoportok és az Azure Active Directory B2B együttműködés |} Microsoft Docs"
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
ms.openlocfilehash: 5818c41610c8c5df89abcb0dcd058bcbe9579ce7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="dynamic-groups-and-azure-active-directory-b2b-collaboration"></a><span data-ttu-id="3e484-103">Dinamikus csoportok és az Azure Active Directory B2B együttműködés</span><span class="sxs-lookup"><span data-stu-id="3e484-103">Dynamic groups and Azure Active Directory B2B collaboration</span></span>

## <a name="what-are-dynamic-groups"></a><span data-ttu-id="3e484-104">Mik azok a dinamikus csoportok?</span><span class="sxs-lookup"><span data-stu-id="3e484-104">What are dynamic groups?</span></span>
<span data-ttu-id="3e484-105">Az Azure Active Directory (Azure AD), biztonsági csoporttagság dinamikus beállítási lehetőségek érhetők el a [az Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="3e484-105">Dynamic configuration of security group membership for Azure Active Directory (Azure AD) is available in [the Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="3e484-106">A rendszergazdák az Azure Active Directory felhasználói attribútumok (például userType, részleg vagy ország) alapján létrehozott csoportok feltöltéséhez szabályainak beállítása.</span><span class="sxs-lookup"><span data-stu-id="3e484-106">Administrators can set rules to populate groups that are created in Azure Active Directory based on user attributes (such as userType, department, or country).</span></span> <span data-ttu-id="3e484-107">Tagok automatikusan lehet hozzáadni vagy az attribútum a biztonsági csoportból eltávolított.</span><span class="sxs-lookup"><span data-stu-id="3e484-107">Members can be automatically added to or removed from a security group based on their attributes.</span></span> <span data-ttu-id="3e484-108">Ezek a csoportok-alkalmazásokra vagy a felhőben található erőforrásokat (SharePoint-webhelyekhez, dokumentumok) és licencek hozzárendelése tagok hozzáférést biztosíthat.</span><span class="sxs-lookup"><span data-stu-id="3e484-108">These groups can provide access to applications or cloud resources (SharePoint sites, documents) and to assign licenses to members.</span></span> <span data-ttu-id="3e484-109">További információk a dinamikus csoportok [dedikált csoportok az Azure Active Directoryban](active-directory-accessmanagement-dedicated-groups.md).</span><span class="sxs-lookup"><span data-stu-id="3e484-109">Read more about dynamic groups in [Dedicated groups in Azure Active Directory](active-directory-accessmanagement-dedicated-groups.md).</span></span>

<span data-ttu-id="3e484-110">A megfelelő [Azure AD Premium P1 és P2 licencelési](https://azure.microsoft.com/pricing/details/active-directory/) létrehozása és dinamikus csoportok használata szükséges.</span><span class="sxs-lookup"><span data-stu-id="3e484-110">The appropriate [Azure AD Premium P1 or P2 licensing](https://azure.microsoft.com/pricing/details/active-directory/) is required to create and use dynamic groups.</span></span> <span data-ttu-id="3e484-111">További információ: a cikk [dinamikus csoporttagság Attribútumalapú szabályok létrehozása az Azure Active Directoryban](active-directory-groups-dynamic-membership-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="3e484-111">Learn more in the article [Create attribute-based rules for dynamic group membership in Azure Active Directory](active-directory-groups-dynamic-membership-azure-portal.md).</span></span>

## <a name="what-are-the-built-in-dynamic-groups"></a><span data-ttu-id="3e484-112">Mik a beépített dinamikus csoportot?</span><span class="sxs-lookup"><span data-stu-id="3e484-112">What are the built-in dynamic groups?</span></span>
<span data-ttu-id="3e484-113">A **minden felhasználó** dinamikus csoport lehetővé teszi, hogy a bérlői rendszergazda egyetlen kattintással a bérlő összes felhasználót tartalmazó csoport létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="3e484-113">The **All users** dynamic group enables tenant admins to create a group containing all users in the tenant with a single click.</span></span> <span data-ttu-id="3e484-114">Alapértelmezés szerint a **minden felhasználó** csoport összes olyan felhasználót tartalmazza a könyvtárban, beleértve a tagok és a vendégek.</span><span class="sxs-lookup"><span data-stu-id="3e484-114">By default, the **All users** group includes all users in the directory, including Members and Guests.</span></span>
<span data-ttu-id="3e484-115">Az új Azure Active Directory felügyeleti portálon, ha szeretné engedélyezni a **minden felhasználó** csoportba, a beállítások nézetben.</span><span class="sxs-lookup"><span data-stu-id="3e484-115">Within the new Azure Active Directory admin portal, you can choose to enable the **All users** group in the Group Settings view.</span></span>

![beépített csoportok](media/active-directory-b2b-dynamic-groups/built-in-groups.png)

## <a name="hardening-the-all-users-dynamic-group"></a><span data-ttu-id="3e484-117">Az összes felhasználó dinamikus csoport korlátozására</span><span class="sxs-lookup"><span data-stu-id="3e484-117">Hardening the All users dynamic group</span></span>
<span data-ttu-id="3e484-118">Alapértelmezés szerint a **minden felhasználó** csoport tartalmazza a B2B együttműködés (vendég) felhasználókat is.</span><span class="sxs-lookup"><span data-stu-id="3e484-118">By default, the **All users** group contains your B2B collaboration (guest) users as well.</span></span> <span data-ttu-id="3e484-119">További biztonságossá teheti a **minden felhasználó** csoport eltávolítása a Vendég szabály segítségével.</span><span class="sxs-lookup"><span data-stu-id="3e484-119">You can further secure your **All users** group by using a rule to remove guest users.</span></span> <span data-ttu-id="3e484-120">A következő ábra azt mutatja be a **minden felhasználó** csoport módosítás vendégek kizárása.</span><span class="sxs-lookup"><span data-stu-id="3e484-120">The following illustration shows the **All users** group modified to exclude guests.</span></span>

![minden felhasználói csoport engedélyezése](media/active-directory-b2b-dynamic-groups/enable-all-users-group.png)

<span data-ttu-id="3e484-122">Is hasznosak lehetnek az új dinamikus csoport, amely tartalmazza azokat a csak a vendégfelhasználók, hogy (például az Azure AD feltételes hozzáférési házirendek) házirendeket is alkalmazhat létrehozása.</span><span class="sxs-lookup"><span data-stu-id="3e484-122">You might also find it useful to create a new dynamic group that contains only guest users, so that you can apply policies (such as Azure AD Conditional Access policies) to them.</span></span>
<span data-ttu-id="3e484-123">Milyen ilyen csoporthoz nézhet ki:</span><span class="sxs-lookup"><span data-stu-id="3e484-123">What such a group might look like:</span></span>

![vendégfelhasználók kizárása](media/active-directory-b2b-dynamic-groups/exclude-guest-users.png)

## <a name="next-steps"></a><span data-ttu-id="3e484-125">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3e484-125">Next steps</span></span>

<span data-ttu-id="3e484-126">Ismerje meg az Azure AD B2B együttműködés további cikkeit:</span><span class="sxs-lookup"><span data-stu-id="3e484-126">Browse our other articles on Azure AD B2B collaboration:</span></span>

* [<span data-ttu-id="3e484-127">Mi az az Azure AD B2B együttműködés?</span><span class="sxs-lookup"><span data-stu-id="3e484-127">What is Azure AD B2B collaboration?</span></span>](active-directory-b2b-what-is-azure-ad-b2b.md)
* [<span data-ttu-id="3e484-128">B2B együttműködés felhasználó tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="3e484-128">B2B collaboration user properties</span></span>](active-directory-b2b-user-properties.md)
* [<span data-ttu-id="3e484-129">Egy szerepkör B2B együttműködés felhasználók hozzáadása</span><span class="sxs-lookup"><span data-stu-id="3e484-129">Adding a B2B collaboration user to a role</span></span>](active-directory-b2b-add-guest-to-role.md)
* [<span data-ttu-id="3e484-130">B2B együttműködés meghívókat delegálása</span><span class="sxs-lookup"><span data-stu-id="3e484-130">Delegate B2B collaboration invitations</span></span>](active-directory-b2b-delegate-invitations.md)
* [<span data-ttu-id="3e484-131">B2B együttműködés kód és a PowerShell-példák</span><span class="sxs-lookup"><span data-stu-id="3e484-131">B2B collaboration code and PowerShell samples</span></span>](active-directory-b2b-code-samples.md)
* [<span data-ttu-id="3e484-132">B2B együttműködés SaaS-alkalmazások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="3e484-132">Configure SaaS apps for B2B collaboration</span></span>](active-directory-b2b-configure-saas-apps.md)
* [<span data-ttu-id="3e484-133">B2B együttműködés felhasználói jogkivonatokhoz</span><span class="sxs-lookup"><span data-stu-id="3e484-133">B2B collaboration user tokens</span></span>](active-directory-b2b-user-token.md)
* [<span data-ttu-id="3e484-134">B2B együttműködés felhasználói jogcímek leképezése</span><span class="sxs-lookup"><span data-stu-id="3e484-134">B2B collaboration user claims mapping</span></span>](active-directory-b2b-claims-mapping.md)
* [<span data-ttu-id="3e484-135">Külső Office 365-megosztás</span><span class="sxs-lookup"><span data-stu-id="3e484-135">Office 365 external sharing</span></span>](active-directory-b2b-o365-external-user.md)
* [<span data-ttu-id="3e484-136">B2B együttműködés aktuális korlátozásai</span><span class="sxs-lookup"><span data-stu-id="3e484-136">B2B collaboration current limitations</span></span>](active-directory-b2b-current-limitations.md)
