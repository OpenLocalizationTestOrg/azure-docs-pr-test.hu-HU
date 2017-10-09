---
title: "aaaAzure feltételes hozzáférést az Active Directory – gyakori kérdések |} Microsoft Docs"
description: "Első válaszok toofrequently kérdések a feltételes hozzáférés az Azure Active Directoryban."
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
ms.openlocfilehash: d23acbb01217d7e9717d1a43de1b46a929404118
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-conditional-access-faqs"></a><span data-ttu-id="d3395-103">Azure Active Directory – gyakori kérdések a feltételes hozzáférés</span><span class="sxs-lookup"><span data-stu-id="d3395-103">Azure Active Directory conditional access FAQs</span></span>

## <a name="which-applications-work-with-conditional-access-policies"></a><span data-ttu-id="d3395-104">Mely alkalmazások használható feltételes hozzáférési szabályzatokat?</span><span class="sxs-lookup"><span data-stu-id="d3395-104">Which applications work with conditional access policies?</span></span>

<span data-ttu-id="d3395-105">A feltételes hozzáférési házirendekkel együtt használható alkalmazások kapcsolatos információkért lásd: [alkalmazások és az Azure Active Directoryban feltételes hozzáférési szabályai használó](active-directory-conditional-access-supported-apps.md).</span><span class="sxs-lookup"><span data-stu-id="d3395-105">For information about applications that work with conditional access policies, see [Applications and browsers that use conditional access rules in Azure Active Directory](active-directory-conditional-access-supported-apps.md).</span></span>

## <a name="are-conditional-access-policies-enforced-for-b2b-collaboration-and-guest-users"></a><span data-ttu-id="d3395-106">Feltételes hozzáférési szabályzatok érvényesítik a B2B együttműködés és a vendégfelhasználók?</span><span class="sxs-lookup"><span data-stu-id="d3395-106">Are conditional access policies enforced for B2B collaboration and guest users?</span></span>

<span data-ttu-id="d3395-107">Üzleti vállalatközi (B2B) együttműködés felhasználók számára a házirendek érvényben vannak.</span><span class="sxs-lookup"><span data-stu-id="d3395-107">Policies are enforced for business-to-business (B2B) collaboration users.</span></span> <span data-ttu-id="d3395-108">Azonban néhány esetben a felhasználó nem feltétlenül tudja toosatisfy hello házirend követelményeinek.</span><span class="sxs-lookup"><span data-stu-id="d3395-108">However, in some cases, a user might not be able toosatisfy hello policy requirements.</span></span> <span data-ttu-id="d3395-109">A Vendég felhasználó szervezet például előfordulhat, hogy támogatja a többtényezős hitelesítés.</span><span class="sxs-lookup"><span data-stu-id="d3395-109">For example, a guest user's organization might not support multi-factor authentication.</span></span> 



## <a name="does-a-sharepoint-online-policy-also-apply-tooonedrive-for-business"></a><span data-ttu-id="d3395-110">A SharePoint Online-szabályzat is vonatkozik tooOneDrive vállalati?</span><span class="sxs-lookup"><span data-stu-id="d3395-110">Does a SharePoint Online policy also apply tooOneDrive for Business?</span></span>

<span data-ttu-id="d3395-111">Igen.</span><span class="sxs-lookup"><span data-stu-id="d3395-111">Yes.</span></span> <span data-ttu-id="d3395-112">A SharePoint Online-szabályzat a vállalati tooOneDrive is vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="d3395-112">A SharePoint Online policy also applies tooOneDrive for Business.</span></span>


## <a name="why-cant-i-set-a-policy-on-client-apps-like-word-or-outlook"></a><span data-ttu-id="d3395-113">Miért nem állítható be egy házirendet az ügyfélalkalmazások, például a Word és Outlook?</span><span class="sxs-lookup"><span data-stu-id="d3395-113">Why can’t I set a policy on client apps, like Word or Outlook?</span></span>

<span data-ttu-id="d3395-114">Feltételes hozzáférési szabályzatot állítja be a szolgáltatások elérésére vonatkozó követelmények.</span><span class="sxs-lookup"><span data-stu-id="d3395-114">A conditional access policy sets requirements for accessing a service.</span></span> <span data-ttu-id="d3395-115">Hitelesítési toothat szolgáltatás esetén van kényszerítve.</span><span class="sxs-lookup"><span data-stu-id="d3395-115">It's enforced when authentication toothat service occurs.</span></span> <span data-ttu-id="d3395-116">hello házirend nincs beállítva közvetlenül egy ügyfélalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="d3395-116">hello policy is not set directly on a client application.</span></span> <span data-ttu-id="d3395-117">Ehelyett azt akkor érvényes, ha egy ügyfél meghívja a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="d3395-117">Instead, it is applied when a client calls a service.</span></span> <span data-ttu-id="d3395-118">A SharePoint beállított házirend például SharePoint hívása tooclients vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="d3395-118">For example, a policy set on SharePoint applies tooclients calling SharePoint.</span></span> <span data-ttu-id="d3395-119">Exchange a beállított házirend tooOutlook vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="d3395-119">A policy set on Exchange applies tooOutlook.</span></span>

## <a name="does-a-conditional-access-policy-apply-tooservice-accounts"></a><span data-ttu-id="d3395-120">Nem érvényes egy feltételes hozzáférési házirend tooservice fiókok?</span><span class="sxs-lookup"><span data-stu-id="d3395-120">Does a conditional access policy apply tooservice accounts?</span></span>

<span data-ttu-id="d3395-121">Feltételes hozzáférési házirendek alkalmazása tooall felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="d3395-121">Conditional access policies apply tooall user accounts.</span></span> <span data-ttu-id="d3395-122">Ez magában foglalja a felhasználói fiókok, szolgáltatásfiókok használatosak.</span><span class="sxs-lookup"><span data-stu-id="d3395-122">This includes user accounts that are used as service accounts.</span></span> <span data-ttu-id="d3395-123">Gyakran a felügyelet nélküli futtató szolgáltatásfiókot nem megfelelnek a hello feltételes hozzáférési házirendet.</span><span class="sxs-lookup"><span data-stu-id="d3395-123">Often, a service account that runs unattended can't satisfy hello requirements of a conditional access policy.</span></span> <span data-ttu-id="d3395-124">Például a multi-factor authentication lehet szükség.</span><span class="sxs-lookup"><span data-stu-id="d3395-124">For example, multi-factor authentication might be required.</span></span> <span data-ttu-id="d3395-125">Szolgáltatásfiókok k zárhatók ki a házirend a feltételes hozzáférési házirend beállítások használatával.</span><span class="sxs-lookup"><span data-stu-id="d3395-125">Service accounts can be excluded from a policy by using conditional access policy management settings.</span></span> 

## <a name="are-graph-apis-available-for-configuring-conditional-access-policies"></a><span data-ttu-id="d3395-126">Graph API-k érhetők el a feltételes hozzáférési szabályzatok konfigurálása?</span><span class="sxs-lookup"><span data-stu-id="d3395-126">Are Graph APIs available for configuring conditional access policies?</span></span>

<span data-ttu-id="d3395-127">Jelenleg nincs.</span><span class="sxs-lookup"><span data-stu-id="d3395-127">Currently, no.</span></span> 

## <a name="what-is-hello-default-exclusion-policy-for-unsupported-device-platforms"></a><span data-ttu-id="d3395-128">Mi az a hello alapértelmezett kizárási házirend nem támogatott eszközplatformok?</span><span class="sxs-lookup"><span data-stu-id="d3395-128">What is hello default exclusion policy for unsupported device platforms?</span></span>

<span data-ttu-id="d3395-129">Feltételes hozzáférési házirendek jelenleg, iOS és Android rendszerű eszközök felhasználóinak szelektív érvényesítik.</span><span class="sxs-lookup"><span data-stu-id="d3395-129">Currently, conditional access policies are selectively enforced on users of iOS and Android devices.</span></span> <span data-ttu-id="d3395-130">Alkalmazások eszköz platformokon, alapértelmezés szerint nem érinti hello feltételes hozzáférési házirend az iOS és Android-eszközökön.</span><span class="sxs-lookup"><span data-stu-id="d3395-130">Applications on other device platforms are, by default, not affected by hello conditional access policy for iOS and Android devices.</span></span> <span data-ttu-id="d3395-131">A bérlői rendszergazda választhat toooverride hello globális házirend toodisallow hozzáférés toousers nem támogatott platformokon.</span><span class="sxs-lookup"><span data-stu-id="d3395-131">A tenant admin can choose toooverride hello global policy toodisallow access toousers on platforms that are not supported.</span></span>


## <a name="how-do-conditional-access-policies-work-for-microsoft-teams"></a><span data-ttu-id="d3395-132">Hogyan működik a Microsoft Teams feltételes hozzáférési szabályzatokat?</span><span class="sxs-lookup"><span data-stu-id="d3395-132">How do conditional access policies work for Microsoft Teams?</span></span>  

<span data-ttu-id="d3395-133">Microsoft Teams erősen támaszkodik a Exchange Online és SharePoint Online core termelékenység forgatókönyvek esetén, mint az értekezletek, naptárak és fájlmegosztás.</span><span class="sxs-lookup"><span data-stu-id="d3395-133">Microsoft Teams relies heavily on Exchange Online and SharePoint Online for core productivity scenarios, like meetings, calendars, and file sharing.</span></span> <span data-ttu-id="d3395-134">Ezekbe a felhőalkalmazásokba beállított feltételes hozzáférési házirendek tooMicrosoft csapatok alkalmazni, amikor egy felhasználó bejelentkezik.</span><span class="sxs-lookup"><span data-stu-id="d3395-134">Conditional access policies that are set for these cloud apps apply tooMicrosoft Teams when a user signs in.</span></span>

<span data-ttu-id="d3395-135">Microsoft Teams is támogatott külön-külön, a felhőalapú alkalmazások az Azure Active Directory feltételes hozzáférési házirendek.</span><span class="sxs-lookup"><span data-stu-id="d3395-135">Microsoft Teams also is supported separately as a cloud app in Azure Active Directory conditional access policies.</span></span> <span data-ttu-id="d3395-136">Felhőalapú alkalmazások beállított tanúsítvány hatóság házirendek tooMicrosoft csapatok alkalmazzák, amikor a felhasználó jelentkezik be.</span><span class="sxs-lookup"><span data-stu-id="d3395-136">Certificate authority policies that are set for a cloud app apply tooMicrosoft Teams when a user signs in.</span></span>

<span data-ttu-id="d3395-137">Microsoft Teams asztali ügyfelek Windows és Mac modern hitelesítést támogatja.</span><span class="sxs-lookup"><span data-stu-id="d3395-137">Microsoft Teams desktop clients for Windows and Mac support modern authentication.</span></span> <span data-ttu-id="d3395-138">Modern hitelesítéssel bejelentkezés hello Azure Active Directory Authentication Library (ADAL) tooMicrosoft Office ügyfélalkalmazások alapján különböző platformokon.</span><span class="sxs-lookup"><span data-stu-id="d3395-138">Modern authentication brings sign-in based on hello Azure Active Directory Authentication Library (ADAL) tooMicrosoft Office client applications across platforms.</span></span> 
