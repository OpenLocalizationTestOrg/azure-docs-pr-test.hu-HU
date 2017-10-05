---
title: "Ajánlott eljárások a feltételes hozzáférés az Azure Active Directoryban |} Microsoft Docs"
description: "További tudnivalók a dolgot tudnia kell, és mi ezzel kerülendő a feltételes hozzáférési szabályzatok konfigurálásakor."
services: active-directory
keywords: "alkalmazások, a feltételes hozzáférés az Azure ad-vel, a biztonságos hozzáférés a vállalati erőforrásokhoz, a feltételes hozzáférési házirendekkel a feltételes hozzáférés"
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/12/2017
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: 3e524c116479c1af6eb6a601c9b57d27a697c5a2
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="best-practices-for-conditional-access-in-azure-active-directory"></a><span data-ttu-id="c63bf-104">Gyakorlati tanácsok a feltételes hozzáférés az Azure Active Directoryban</span><span class="sxs-lookup"><span data-stu-id="c63bf-104">Best practices for conditional access in Azure Active Directory</span></span>

<span data-ttu-id="c63bf-105">Ez a témakör a dolgot tudnia kell, és mi ezzel kerülendő a feltételes hozzáférési szabályzatok konfigurálásakor kapcsolatos információkat nyújt.</span><span class="sxs-lookup"><span data-stu-id="c63bf-105">This topic provides you with information about things you should know and what it is you should avoid doing when configuring conditional access policies.</span></span> <span data-ttu-id="c63bf-106">Ebben a témakörben olvasásakor, előtt meg kell Ismerkedjen meg a fogalmakat és terminológiával leírt [feltételes hozzáférés az Azure Active Directoryban](active-directory-conditional-access-azure-portal.md)</span><span class="sxs-lookup"><span data-stu-id="c63bf-106">Before reading this topic, you should familiarize yourself with the concepts and the terminology outlined in [Conditional access in Azure Active Directory](active-directory-conditional-access-azure-portal.md)</span></span>

## <a name="what-you-should-know"></a><span data-ttu-id="c63bf-107">Tudnivalók</span><span class="sxs-lookup"><span data-stu-id="c63bf-107">What you should know</span></span>

### <a name="whats-required-to-make-a-policy-work"></a><span data-ttu-id="c63bf-108">Mi szükséges munka házirendet?</span><span class="sxs-lookup"><span data-stu-id="c63bf-108">What’s required to make a policy work?</span></span>

<span data-ttu-id="c63bf-109">Ha egy új házirendet hoz létre, nincsenek felhasználók, csoportok, alkalmazások vagy kiválasztott hozzáférés-vezérlést.</span><span class="sxs-lookup"><span data-stu-id="c63bf-109">When you create a new policy, there are no users, groups, apps or access controls selected.</span></span>

![Felhőalkalmazások](./media/active-directory-conditional-access-best-practices/02.png)


<span data-ttu-id="c63bf-111">Ahhoz, hogy a házirend működik, konfigurálnia kell a következő:</span><span class="sxs-lookup"><span data-stu-id="c63bf-111">To make your policy work, you must configure the following:</span></span>


|<span data-ttu-id="c63bf-112">mi</span><span class="sxs-lookup"><span data-stu-id="c63bf-112">What</span></span>           | <span data-ttu-id="c63bf-113">Hogyan</span><span class="sxs-lookup"><span data-stu-id="c63bf-113">How</span></span>                                  | <span data-ttu-id="c63bf-114">miért</span><span class="sxs-lookup"><span data-stu-id="c63bf-114">Why</span></span>|
|:--            | :--                                  | :-- |
|<span data-ttu-id="c63bf-115">**Felhőalkalmazások**</span><span class="sxs-lookup"><span data-stu-id="c63bf-115">**Cloud apps**</span></span> |<span data-ttu-id="c63bf-116">Válasszon egy vagy több alkalmazás van szüksége.</span><span class="sxs-lookup"><span data-stu-id="c63bf-116">You need to select one or more apps.</span></span>  | <span data-ttu-id="c63bf-117">A feltételes hozzáférési házirend célja az, hogy lehetővé teszik annak finomhangolásához, hogy hogyan engedéllyel rendelkező felhasználók férhetnek hozzá az alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="c63bf-117">The goal of a conditional access policy is to enable you to fine-tune how authorized users can access your apps.</span></span>|
| <span data-ttu-id="c63bf-118">**Felhasználók és csoportok**</span><span class="sxs-lookup"><span data-stu-id="c63bf-118">**Users and groups**</span></span> | <span data-ttu-id="c63bf-119">Válasszon legalább egy felhasználót vagy csoportot, amely a kijelölt felhőalapú alkalmazások elérésére jogosult kell.</span><span class="sxs-lookup"><span data-stu-id="c63bf-119">You need to select at least one user or group that is authorized to access the cloud apps you have selected.</span></span> | <span data-ttu-id="c63bf-120">A feltételes hozzáférési szabályzat, amelynek nincs hozzárendelt felhasználók és csoportok, soha nem aktiválódott.</span><span class="sxs-lookup"><span data-stu-id="c63bf-120">A conditional access policy that has no users and groups assigned, is never triggered.</span></span> |
| <span data-ttu-id="c63bf-121">**Hozzáférés-vezérlést**</span><span class="sxs-lookup"><span data-stu-id="c63bf-121">**Access controls**</span></span> | <span data-ttu-id="c63bf-122">Válasszon legalább egy hozzáférés-vezérlés kell.</span><span class="sxs-lookup"><span data-stu-id="c63bf-122">You need to select at least one access control.</span></span> | <span data-ttu-id="c63bf-123">A házirend processzor tudnia kell, mi a teendő, ha a feltételek teljesülnek.</span><span class="sxs-lookup"><span data-stu-id="c63bf-123">Your policy processor needs to know what to do if your conditions are satisfied.</span></span>|


<span data-ttu-id="c63bf-124">Ezen alapszintű követelményeken sok esetben is konfigurálnia kell egy feltételt.</span><span class="sxs-lookup"><span data-stu-id="c63bf-124">In addition to these basic requirements, in many cases, you should also configure a condition.</span></span> <span data-ttu-id="c63bf-125">Házirend akkor is megvalósítható, ha egy konfigurált feltételt, amíg a feltételek közül mind a vezetői tényező a pontosabb beállításra a alkalmazásokhoz való hozzáférés.</span><span class="sxs-lookup"><span data-stu-id="c63bf-125">While a policy would also work without a configured condition, conditions are the driving factor for fine-tuning access to your apps.</span></span>


![Felhőalkalmazások](./media/active-directory-conditional-access-best-practices/04.png)



### <a name="how-are-assignments-evaluated"></a><span data-ttu-id="c63bf-127">Hogyan hozzárendelések értékeli ki a rendszer?</span><span class="sxs-lookup"><span data-stu-id="c63bf-127">How are assignments evaluated?</span></span>

<span data-ttu-id="c63bf-128">Minden hozzárendeléseket a rendszer logikailag **műveletet**.</span><span class="sxs-lookup"><span data-stu-id="c63bf-128">All assignments are logically **ANDed**.</span></span> <span data-ttu-id="c63bf-129">Ha egynél több hozzárendelés konfigurálva van, a házirend elindítása vonatkozó összes hozzárendelést kell biztosítani.</span><span class="sxs-lookup"><span data-stu-id="c63bf-129">If you have more than one assignment configured, to trigger a policy, all assignments must be satisfied.</span></span>  

<span data-ttu-id="c63bf-130">Ha a vállalati hálózaton kívül létesített összes kapcsolat alkalmazó hely feltétel beállítása kell, ez által elvégezhető:</span><span class="sxs-lookup"><span data-stu-id="c63bf-130">If you need to configure a location condition that applies to all connections made from outside your organization's network, you can accomplish this by:</span></span>

- <span data-ttu-id="c63bf-131">Beleértve **az összes hely**</span><span class="sxs-lookup"><span data-stu-id="c63bf-131">Including **All locations**</span></span>
- <span data-ttu-id="c63bf-132">Kivéve **a megbízható IP-címek**</span><span class="sxs-lookup"><span data-stu-id="c63bf-132">Excluding **All trusted IPs**</span></span>

### <a name="what-happens-if-you-have-policies-in-the-azure-classic-portal-and-azure-portal-configured"></a><span data-ttu-id="c63bf-133">Mi történik, ha a klasszikus Azure portál és a konfigurált Azure-portálon házirendek?</span><span class="sxs-lookup"><span data-stu-id="c63bf-133">What happens if you have policies in the Azure classic portal and Azure portal configured?</span></span>  
<span data-ttu-id="c63bf-134">Azure Active Directory mindkét házirend érvényesíti, és a felhasználó élvezheti a hozzáférést, csak akkor, ha összes követelmény teljesülését.</span><span class="sxs-lookup"><span data-stu-id="c63bf-134">Both policies are enforced by Azure Active Directory and the user gets access only when all requirements are met.</span></span>

### <a name="what-happens-if-you-have-policies-in-the-intune-silverlight-portal-and-the-azure-portal"></a><span data-ttu-id="c63bf-135">Mi történik, ha az Intune Silverlight és az Azure-portálhoz a házirendek?</span><span class="sxs-lookup"><span data-stu-id="c63bf-135">What happens if you have policies in the Intune Silverlight portal and the Azure Portal?</span></span>
<span data-ttu-id="c63bf-136">Azure Active Directory mindkét házirend érvényesíti, és a felhasználó élvezheti a hozzáférést, csak akkor, ha összes követelmény teljesülését.</span><span class="sxs-lookup"><span data-stu-id="c63bf-136">Both policies are enforced by Azure Active Directory and the user gets access only when all requirements are met.</span></span>

### <a name="what-happens-if-i-have-multiple-policies-for-the-same-user-configured"></a><span data-ttu-id="c63bf-137">Mi történik, ha a konfigurált felhasználónak több házirendet?</span><span class="sxs-lookup"><span data-stu-id="c63bf-137">What happens if I have multiple policies for the same user configured?</span></span>  
<span data-ttu-id="c63bf-138">Minden bejelentkezés az Azure Active Directory kiértékeli az összes házirend, és biztosítja, hogy az összes követelmények teljesülnek-e a felhasználó hozzáférése engedélyezett előtt.</span><span class="sxs-lookup"><span data-stu-id="c63bf-138">For every sign-in, Azure Active Directory evaluates all policies and ensures that all requirements are met before granted access to the user.</span></span>


### <a name="does-conditional-access-work-with-exchange-activesync"></a><span data-ttu-id="c63bf-139">Feltételes hozzáférés az Exchange ActiveSync szolgáltatással működik?</span><span class="sxs-lookup"><span data-stu-id="c63bf-139">Does conditional access work with Exchange ActiveSync?</span></span>

<span data-ttu-id="c63bf-140">Igen, az Exchange ActiveSync is használhatja a feltételes hozzáférési házirendben.</span><span class="sxs-lookup"><span data-stu-id="c63bf-140">Yes, you can use Exchange ActiveSync in a conditional access policy.</span></span>


## <a name="what-you-should-avoid-doing"></a><span data-ttu-id="c63bf-141">Mi kerülendő ezzel</span><span class="sxs-lookup"><span data-stu-id="c63bf-141">What you should avoid doing</span></span>

<span data-ttu-id="c63bf-142">A feltételes hozzáférés keretrendszer egy nagyszerű konfigurációs rugalmasságot biztosít.</span><span class="sxs-lookup"><span data-stu-id="c63bf-142">The conditional access framework provides you with a great configuration flexibility.</span></span> <span data-ttu-id="c63bf-143">Azonban rugalmas lehetőségeket biztosítanak azt is jelenti, hogy alaposan tekintse át minden konfigurációs házirend nem kívánt eredmények elkerülése érdekében annak engedélyezése előtt.</span><span class="sxs-lookup"><span data-stu-id="c63bf-143">However, great flexibility  also means that you should carefully review each configuration policy prior to releasing it to avoid undesirable results.</span></span> <span data-ttu-id="c63bf-144">Ebben a környezetben, akkor kell nagy figyelmet fordítani, mint a teljes érintő hozzárendelések **minden felhasználók / csoportok / felhőalapú alkalmazásokba**.</span><span class="sxs-lookup"><span data-stu-id="c63bf-144">In this context, you should pay special attention to assignments affecting complete sets such as **all users / groups / cloud apps**.</span></span>

<span data-ttu-id="c63bf-145">A környezetben kerülje el a következő beállításokat:</span><span class="sxs-lookup"><span data-stu-id="c63bf-145">In your environment, you should avoid the following configurations:</span></span>


<span data-ttu-id="c63bf-146">**Az összes felhasználó számára az összes felhőalapú alkalmazásokat:**</span><span class="sxs-lookup"><span data-stu-id="c63bf-146">**For all users, all cloud apps:**</span></span>

- <span data-ttu-id="c63bf-147">**Letiltja a hozzáférést,** -ebben a konfigurációban a teljes szervezet, amely nincs mindenképpen célszerű blokkolja.</span><span class="sxs-lookup"><span data-stu-id="c63bf-147">**Block access** - This configuration blocks your entire organization, which is definitely not a good idea.</span></span>

- <span data-ttu-id="c63bf-148">**Megfelelő eszközökre szükséges** – a felhasználók számára, amely nem regisztrálták az eszközeiket, mégis a házirend nem engedélyezi az összes eléréséhez, beleértve az Intune-portál eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="c63bf-148">**Require compliant device** - For users that don't have enrolled their devices yet, this policy blocks all access including access to the Intune portal.</span></span> <span data-ttu-id="c63bf-149">Ha Ön rendszergazda a regisztrált eszköz nélküli, akkor ez a házirend letiltja a illetéktelen vissza az Azure-portálon a házirend módosítása.</span><span class="sxs-lookup"><span data-stu-id="c63bf-149">If you are an administrator without an enrolled device, this policy blocks you from getting back into the Azure portal to change the policy.</span></span>

- <span data-ttu-id="c63bf-150">**Megkövetelje a tartományhoz való csatlakozást** – Ez a házirend-blokk hozzáférés nem is letiltja a hozzáférést a szervezete összes felhasználója számára, ha még nem rendelkezik a tartományhoz csatlakoztatott eszközön.</span><span class="sxs-lookup"><span data-stu-id="c63bf-150">**Require domain join** - This policy block access has also the potential to block access for all users in your organization if you don't have a domain-joined device yet.</span></span>


<span data-ttu-id="c63bf-151">**Az összes felhasználó számára az összes felhőalapú alkalmazások, az összes eszközplatformra:**</span><span class="sxs-lookup"><span data-stu-id="c63bf-151">**For all users, all cloud apps, all device platforms:**</span></span>

- <span data-ttu-id="c63bf-152">**Letiltja a hozzáférést,** -ebben a konfigurációban a teljes szervezet, amely nincs mindenképpen célszerű blokkolja.</span><span class="sxs-lookup"><span data-stu-id="c63bf-152">**Block access** - This configuration blocks your entire organization, which is definitely not a good idea.</span></span>


## <a name="common-scenarios"></a><span data-ttu-id="c63bf-153">Gyakori forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="c63bf-153">Common scenarios</span></span>

### <a name="requiring-multi-factor-authentication-for-apps"></a><span data-ttu-id="c63bf-154">Többtényezős hitelesítés megkövetelése az alkalmazások</span><span class="sxs-lookup"><span data-stu-id="c63bf-154">Requiring multi-factor authentication for apps</span></span>

<span data-ttu-id="c63bf-155">Sok környezetben a többinél magasabb szintű védelmet igénylő alkalmazások rendelkeznek.</span><span class="sxs-lookup"><span data-stu-id="c63bf-155">Many environments have apps requiring a higher level of protection than the others.</span></span>
<span data-ttu-id="c63bf-156">Ez helyzet, például az alkalmazások, amelyekre a bizalmas adatokhoz való hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="c63bf-156">This is, for example, the case for apps that have access to sensitive data.</span></span>
<span data-ttu-id="c63bf-157">Ha a kívánt védelmi réteget hozzá ezekhez az alkalmazásokhoz, konfigurálhatja egy feltételes hozzáférési szabályzatot, amely többtényezős hitelesítést igényel, amikor a felhasználók elérik-e ezeket az alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="c63bf-157">If you want to add another layer of protection to these apps, you can configure a conditional access policy that requires multi-factor authentication when users are accessing these apps.</span></span>


### <a name="requiring-multi-factor-authentication-for-access-from-networks-that-are-not-trusted"></a><span data-ttu-id="c63bf-158">Többtényezős hitelesítés megkövetelése a nem megbízható hálózatokhoz való hozzáférést</span><span class="sxs-lookup"><span data-stu-id="c63bf-158">Requiring multi-factor authentication for access from networks that are not trusted</span></span>

<span data-ttu-id="c63bf-159">Ebben a forgatókönyvben nem hasonló az előző helyzethez, mert a multi-factor authentication követelmény hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="c63bf-159">This scenario is similar to the previous scenario because it adds a requirement for multi-factor authentication.</span></span>
<span data-ttu-id="c63bf-160">A fő különbség azonban ez a követelmény feltételét.</span><span class="sxs-lookup"><span data-stu-id="c63bf-160">However, the main difference is the condition for this requirement.</span></span>  
<span data-ttu-id="c63bf-161">Míg az előző példában a fókuszában sensitve adatokhoz való hozzáférés alkalmazások volt, az ebben a forgatókönyvben elsősorban megbízható helyeken.</span><span class="sxs-lookup"><span data-stu-id="c63bf-161">While the focus of the previous scenario was on apps with access to sensitve data, the focus of this scenario is on trusted locations.</span></span>  
<span data-ttu-id="c63bf-162">Ez azt jelenti lehetséges, hogy a multi-factor authentication követelmény az alkalmazások a felhasználó nem megbízható hálózaton keresztül illetéktelen.</span><span class="sxs-lookup"><span data-stu-id="c63bf-162">In other words, you might have a requirement for multi-factor authentication if an app is accessed by a user from a network you don't trust.</span></span>


### <a name="only-trusted-devices-can-access-office-365-services"></a><span data-ttu-id="c63bf-163">Csak a megbízható eszközök férhetnek hozzá az Office 365-szolgáltatásokhoz</span><span class="sxs-lookup"><span data-stu-id="c63bf-163">Only trusted devices can access Office 365 services</span></span>

<span data-ttu-id="c63bf-164">Intune használ a környezetben, ha azonnal megkezdheti a feltételes hozzáférési házirendek felülete a Azure-konzolon.</span><span class="sxs-lookup"><span data-stu-id="c63bf-164">If you are using Intune in your environment, you can immediately start using the conditional access policy interface in the Azure console.</span></span>

<span data-ttu-id="c63bf-165">Számos Intune-ügyfél számára a feltételes hozzáférés segítségével győződjön meg arról, hogy csak a megbízható eszközök férhetnek hozzá az Office 365-szolgáltatásokhoz.</span><span class="sxs-lookup"><span data-stu-id="c63bf-165">Many Intune customers are using conditional access to ensure that only trusted devices can access Office 365 services.</span></span> <span data-ttu-id="c63bf-166">Ez azt jelenti, hogy a mobileszközök az Intune-nal beléptetett és megfelelőségi házirend követelményeknek, és, hogy a Windows rendszerű számítógépek csatlakozik egy helyszíni tartományban.</span><span class="sxs-lookup"><span data-stu-id="c63bf-166">This means that mobile devices are enrolled with Intune and meet compliance policy requirements, and that Windows PCs are joined to an on-premises domain.</span></span> <span data-ttu-id="c63bf-167">A kulcs fokozása, nem kell ugyanabban a házirendben beállítása az egyes az Office 365-szolgáltatásokhoz.</span><span class="sxs-lookup"><span data-stu-id="c63bf-167">A key improvement is that you do not have to set the same policy for each of the Office 365 services.</span></span>  <span data-ttu-id="c63bf-168">Ha egy új házirendet hoz létre, konfigurálja az egyes az Office 365-alkalmazásokat, amelyek feltételes hozzáférésű védeni kíván felvenni a felhőalapú alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="c63bf-168">When you create a new policy, configure the Cloud apps to include each of the O365 apps that you wish to protect with  with Conditional Access.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c63bf-169">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c63bf-169">Next steps</span></span>

<span data-ttu-id="c63bf-170">Ha meg szeretné ismerni a feltételes hozzáférési házirend konfigurálása tudnivalókat [Ismerkedés a feltételes hozzáférés az Azure Active Directoryban](active-directory-conditional-access-azure-portal-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="c63bf-170">If you want to know how to configure a conditional access policy, see [Get started with conditional access in Azure Active Directory](active-directory-conditional-access-azure-portal-get-started.md).</span></span>
