---
title: "Azure Active Directory feltételes hozzáférés – gyakori kérdések |} Microsoft Docs"
description: "Válaszok feltételes hozzáférésével kapcsolatos gyakori kérdések az Azure Active Directoryban."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 14f7fc83-f4bb-41bf-b6f1-a9bb97717c34
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: e9a5af41b08b593e4d97475f29da4e5fe8df7042
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="azure-active-directory-conditional-access-faqs"></a><span data-ttu-id="e9040-103">Azure Active Directory – gyakori kérdések a feltételes hozzáférés</span><span class="sxs-lookup"><span data-stu-id="e9040-103">Azure Active Directory conditional access FAQs</span></span>

## <a name="which-applications-work-with-conditional-access-policies"></a><span data-ttu-id="e9040-104">Mely alkalmazások használható feltételes hozzáférési szabályzatokat?</span><span class="sxs-lookup"><span data-stu-id="e9040-104">Which applications work with conditional access policies?</span></span>

<span data-ttu-id="e9040-105">A feltételes hozzáférési házirendekkel együtt használható alkalmazások kapcsolatos információkért lásd: [alkalmazások és az Azure Active Directoryban feltételes hozzáférési szabályai használó](active-directory-conditional-access-supported-apps.md).</span><span class="sxs-lookup"><span data-stu-id="e9040-105">For information about applications that work with conditional access policies, see [Applications and browsers that use conditional access rules in Azure Active Directory](active-directory-conditional-access-supported-apps.md).</span></span>

## <a name="are-conditional-access-policies-enforced-for-b2b-collaboration-and-guest-users"></a><span data-ttu-id="e9040-106">Feltételes hozzáférési szabályzatok érvényesítik a B2B együttműködés és a vendégfelhasználók?</span><span class="sxs-lookup"><span data-stu-id="e9040-106">Are conditional access policies enforced for B2B collaboration and guest users?</span></span>

<span data-ttu-id="e9040-107">Üzleti vállalatközi (B2B) együttműködés felhasználók számára a házirendek érvényben vannak.</span><span class="sxs-lookup"><span data-stu-id="e9040-107">Policies are enforced for business-to-business (B2B) collaboration users.</span></span> <span data-ttu-id="e9040-108">Azonban néhány esetben a felhasználó nem feltétlenül teljesíteni a házirend követelményeinek.</span><span class="sxs-lookup"><span data-stu-id="e9040-108">However, in some cases, a user might not be able to satisfy the policy requirements.</span></span> <span data-ttu-id="e9040-109">A Vendég felhasználó szervezet például előfordulhat, hogy támogatja a többtényezős hitelesítés.</span><span class="sxs-lookup"><span data-stu-id="e9040-109">For example, a guest user's organization might not support multi-factor authentication.</span></span> 



## <a name="does-a-sharepoint-online-policy-also-apply-to-onedrive-for-business"></a><span data-ttu-id="e9040-110">A SharePoint Online-szabályzat is vonatkozik onedrive vállalati verzió?</span><span class="sxs-lookup"><span data-stu-id="e9040-110">Does a SharePoint Online policy also apply to OneDrive for Business?</span></span>

<span data-ttu-id="e9040-111">Igen.</span><span class="sxs-lookup"><span data-stu-id="e9040-111">Yes.</span></span> <span data-ttu-id="e9040-112">A SharePoint Online-házirend a onedrive vállalati verzió is vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="e9040-112">A SharePoint Online policy also applies to OneDrive for Business.</span></span>


## <a name="why-cant-i-set-a-policy-on-client-apps-like-word-or-outlook"></a><span data-ttu-id="e9040-113">Miért nem állítható be egy házirendet az ügyfélalkalmazások, például a Word és Outlook?</span><span class="sxs-lookup"><span data-stu-id="e9040-113">Why can’t I set a policy on client apps, like Word or Outlook?</span></span>

<span data-ttu-id="e9040-114">Feltételes hozzáférési szabályzatot állítja be a szolgáltatások elérésére vonatkozó követelmények.</span><span class="sxs-lookup"><span data-stu-id="e9040-114">A conditional access policy sets requirements for accessing a service.</span></span> <span data-ttu-id="e9040-115">Az adott szolgáltatáshoz hitelesítés esetén van kényszerítve.</span><span class="sxs-lookup"><span data-stu-id="e9040-115">It's enforced when authentication to that service occurs.</span></span> <span data-ttu-id="e9040-116">A házirend nincs beállítva közvetlenül egy ügyfélalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="e9040-116">The policy is not set directly on a client application.</span></span> <span data-ttu-id="e9040-117">Ehelyett azt akkor érvényes, ha egy ügyfél meghívja a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="e9040-117">Instead, it is applied when a client calls a service.</span></span> <span data-ttu-id="e9040-118">A SharePoint beállított házirend például SharePoint hívó ügyfelek vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="e9040-118">For example, a policy set on SharePoint applies to clients calling SharePoint.</span></span> <span data-ttu-id="e9040-119">Exchange a beállított házirend Outlook vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="e9040-119">A policy set on Exchange applies to Outlook.</span></span>

## <a name="does-a-conditional-access-policy-apply-to-service-accounts"></a><span data-ttu-id="e9040-120">A feltételes hozzáférési házirend vonatkozik szolgáltatásfiókok?</span><span class="sxs-lookup"><span data-stu-id="e9040-120">Does a conditional access policy apply to service accounts?</span></span>

<span data-ttu-id="e9040-121">Feltételes hozzáférési házirendek felhasználói fiókokhoz vonatkoznak.</span><span class="sxs-lookup"><span data-stu-id="e9040-121">Conditional access policies apply to all user accounts.</span></span> <span data-ttu-id="e9040-122">Ez magában foglalja a felhasználói fiókok, szolgáltatásfiókok használatosak.</span><span class="sxs-lookup"><span data-stu-id="e9040-122">This includes user accounts that are used as service accounts.</span></span> <span data-ttu-id="e9040-123">Gyakran a felügyelet nélküli futtató szolgáltatásfiókot nem felel meg a feltételes hozzáférési házirendet.</span><span class="sxs-lookup"><span data-stu-id="e9040-123">Often, a service account that runs unattended can't satisfy the requirements of a conditional access policy.</span></span> <span data-ttu-id="e9040-124">Például a multi-factor authentication lehet szükség.</span><span class="sxs-lookup"><span data-stu-id="e9040-124">For example, multi-factor authentication might be required.</span></span> <span data-ttu-id="e9040-125">Szolgáltatásfiókok k zárhatók ki a házirend a feltételes hozzáférési házirend beállítások használatával.</span><span class="sxs-lookup"><span data-stu-id="e9040-125">Service accounts can be excluded from a policy by using conditional access policy management settings.</span></span> 

## <a name="are-graph-apis-available-for-configuring-conditional-access-policies"></a><span data-ttu-id="e9040-126">Graph API-k érhetők el a feltételes hozzáférési szabályzatok konfigurálása?</span><span class="sxs-lookup"><span data-stu-id="e9040-126">Are Graph APIs available for configuring conditional access policies?</span></span>

<span data-ttu-id="e9040-127">Jelenleg nincs.</span><span class="sxs-lookup"><span data-stu-id="e9040-127">Currently, no.</span></span> 

## <a name="what-is-the-default-exclusion-policy-for-unsupported-device-platforms"></a><span data-ttu-id="e9040-128">Mi az az alapértelmezett kizárási házirend nem támogatott eszközplatformok?</span><span class="sxs-lookup"><span data-stu-id="e9040-128">What is the default exclusion policy for unsupported device platforms?</span></span>

<span data-ttu-id="e9040-129">Feltételes hozzáférési házirendek jelenleg, iOS és Android rendszerű eszközök felhasználóinak szelektív érvényesítik.</span><span class="sxs-lookup"><span data-stu-id="e9040-129">Currently, conditional access policies are selectively enforced on users of iOS and Android devices.</span></span> <span data-ttu-id="e9040-130">Alkalmazások eszköz platformokon, alapértelmezés szerint nem érinti a feltételes hozzáférési házirend az iOS és Android-eszközökön.</span><span class="sxs-lookup"><span data-stu-id="e9040-130">Applications on other device platforms are, by default, not affected by the conditional access policy for iOS and Android devices.</span></span> <span data-ttu-id="e9040-131">Egy Bérlői rendszergazda megadhat bírálja felül a globális házirend nem engedélyezi a hozzáférést a felhasználók által nem támogatott platformokon.</span><span class="sxs-lookup"><span data-stu-id="e9040-131">A tenant admin can choose to override the global policy to disallow access to users on platforms that are not supported.</span></span>


## <a name="how-do-conditional-access-policies-work-for-microsoft-teams"></a><span data-ttu-id="e9040-132">Hogyan működik a Microsoft Teams feltételes hozzáférési szabályzatokat?</span><span class="sxs-lookup"><span data-stu-id="e9040-132">How do conditional access policies work for Microsoft Teams?</span></span>  

<span data-ttu-id="e9040-133">Microsoft Teams erősen támaszkodik a Exchange Online és SharePoint Online core termelékenység forgatókönyvek esetén, mint az értekezletek, naptárak és fájlmegosztás.</span><span class="sxs-lookup"><span data-stu-id="e9040-133">Microsoft Teams relies heavily on Exchange Online and SharePoint Online for core productivity scenarios, like meetings, calendars, and file sharing.</span></span> <span data-ttu-id="e9040-134">Ezekbe a felhőalkalmazásokba beállított feltételes hozzáférési házirendek Microsoft Teams vonatkozik, amikor egy felhasználó bejelentkezik.</span><span class="sxs-lookup"><span data-stu-id="e9040-134">Conditional access policies that are set for these cloud apps apply to Microsoft Teams when a user signs in.</span></span>

<span data-ttu-id="e9040-135">Microsoft Teams is támogatott külön-külön, a felhőalapú alkalmazások az Azure Active Directory feltételes hozzáférési házirendek.</span><span class="sxs-lookup"><span data-stu-id="e9040-135">Microsoft Teams also is supported separately as a cloud app in Azure Active Directory conditional access policies.</span></span> <span data-ttu-id="e9040-136">Felhőalapú alkalmazások beállított tanúsítvány hatóság házirendek Microsoft Teams vonatkozik, amikor egy felhasználó bejelentkezik.</span><span class="sxs-lookup"><span data-stu-id="e9040-136">Certificate authority policies that are set for a cloud app apply to Microsoft Teams when a user signs in.</span></span>

<span data-ttu-id="e9040-137">Microsoft Teams asztali ügyfelek Windows és Mac modern hitelesítést támogatja.</span><span class="sxs-lookup"><span data-stu-id="e9040-137">Microsoft Teams desktop clients for Windows and Mac support modern authentication.</span></span> <span data-ttu-id="e9040-138">Modern hitelesítéssel bejelentkezhet a az Azure Active Directory hitelesítési könyvtár (ADAL) a Microsoft Office ügyfélalkalmazások alapú különböző platformokon.</span><span class="sxs-lookup"><span data-stu-id="e9040-138">Modern authentication brings sign-in based on the Azure Active Directory Authentication Library (ADAL) to Microsoft Office client applications across platforms.</span></span> 