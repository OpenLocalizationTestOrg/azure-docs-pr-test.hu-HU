---
title: "feltételes hozzáférés az Azure Active Directoryban aaaBest eljárásai |} Microsoft Docs"
description: "További tudnivalók a dolgot tudnia kell, és mi ezzel kerülendő a feltételes hozzáférési szabályzatok konfigurálásakor."
services: active-directory
keywords: "feltételes hozzáférés tooapps, feltételes hozzáférés az Azure AD-vel biztonságos hozzáférés toocompany erőforrásokat, a feltételes hozzáférési házirendek"
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
ms.openlocfilehash: 4952f8746a2e583380b3bb99cfe2fbdae1c07b08
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="best-practices-for-conditional-access-in-azure-active-directory"></a><span data-ttu-id="15cab-104">Gyakorlati tanácsok a feltételes hozzáférés az Azure Active Directoryban</span><span class="sxs-lookup"><span data-stu-id="15cab-104">Best practices for conditional access in Azure Active Directory</span></span>

<span data-ttu-id="15cab-105">Ez a témakör a dolgot tudnia kell, és mi ezzel kerülendő a feltételes hozzáférési szabályzatok konfigurálásakor kapcsolatos információkat nyújt.</span><span class="sxs-lookup"><span data-stu-id="15cab-105">This topic provides you with information about things you should know and what it is you should avoid doing when configuring conditional access policies.</span></span> <span data-ttu-id="15cab-106">Ebben a témakörben olvasásakor, előtt tanulmányozza át kapcsolatos hello fogalmakat és hello terminológia leírt [feltételes hozzáférés az Azure Active Directoryban](active-directory-conditional-access-azure-portal.md)</span><span class="sxs-lookup"><span data-stu-id="15cab-106">Before reading this topic, you should familiarize yourself with hello concepts and hello terminology outlined in [Conditional access in Azure Active Directory](active-directory-conditional-access-azure-portal.md)</span></span>

## <a name="what-you-should-know"></a><span data-ttu-id="15cab-107">Tudnivalók</span><span class="sxs-lookup"><span data-stu-id="15cab-107">What you should know</span></span>

### <a name="whats-required-toomake-a-policy-work"></a><span data-ttu-id="15cab-108">Mi szükséges toomake házirend munkahelyi?</span><span class="sxs-lookup"><span data-stu-id="15cab-108">What’s required toomake a policy work?</span></span>

<span data-ttu-id="15cab-109">Ha egy új házirendet hoz létre, nincsenek felhasználók, csoportok, alkalmazások vagy kiválasztott hozzáférés-vezérlést.</span><span class="sxs-lookup"><span data-stu-id="15cab-109">When you create a new policy, there are no users, groups, apps or access controls selected.</span></span>

![Felhőalkalmazások](./media/active-directory-conditional-access-best-practices/02.png)


<span data-ttu-id="15cab-111">toomake a házirend működik, konfigurálnia kell a következő hello:</span><span class="sxs-lookup"><span data-stu-id="15cab-111">toomake your policy work, you must configure hello following:</span></span>


|<span data-ttu-id="15cab-112">mi</span><span class="sxs-lookup"><span data-stu-id="15cab-112">What</span></span>           | <span data-ttu-id="15cab-113">Hogyan</span><span class="sxs-lookup"><span data-stu-id="15cab-113">How</span></span>                                  | <span data-ttu-id="15cab-114">miért</span><span class="sxs-lookup"><span data-stu-id="15cab-114">Why</span></span>|
|:--            | :--                                  | :-- |
|<span data-ttu-id="15cab-115">**Felhőalkalmazások**</span><span class="sxs-lookup"><span data-stu-id="15cab-115">**Cloud apps**</span></span> |<span data-ttu-id="15cab-116">Tooselect kell egy vagy több alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="15cab-116">You need tooselect one or more apps.</span></span>  | <span data-ttu-id="15cab-117">hello feltételes hozzáférési házirend célja tooenable meg toofine-hangolási hogyan engedéllyel rendelkező felhasználók férhetnek hozzá az alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="15cab-117">hello goal of a conditional access policy is tooenable you toofine-tune how authorized users can access your apps.</span></span>|
| <span data-ttu-id="15cab-118">**Felhasználók és csoportok**</span><span class="sxs-lookup"><span data-stu-id="15cab-118">**Users and groups**</span></span> | <span data-ttu-id="15cab-119">Szüksége tooselect legalább egy felhasználót vagy csoportot, amely jogosult a kiválasztott tooaccess hello felhőalapú alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="15cab-119">You need tooselect at least one user or group that is authorized tooaccess hello cloud apps you have selected.</span></span> | <span data-ttu-id="15cab-120">A feltételes hozzáférési szabályzat, amelynek nincs hozzárendelt felhasználók és csoportok, soha nem aktiválódott.</span><span class="sxs-lookup"><span data-stu-id="15cab-120">A conditional access policy that has no users and groups assigned, is never triggered.</span></span> |
| <span data-ttu-id="15cab-121">**Hozzáférés-vezérlést**</span><span class="sxs-lookup"><span data-stu-id="15cab-121">**Access controls**</span></span> | <span data-ttu-id="15cab-122">Legalább egy tooselect kell hozzáférés-vezérlést.</span><span class="sxs-lookup"><span data-stu-id="15cab-122">You need tooselect at least one access control.</span></span> | <span data-ttu-id="15cab-123">A házirend processzor kell tooknow milyen toodo a feltételek teljesülése esetén.</span><span class="sxs-lookup"><span data-stu-id="15cab-123">Your policy processor needs tooknow what toodo if your conditions are satisfied.</span></span>|


<span data-ttu-id="15cab-124">Továbbá toothese alapvető követelményeivel, sok esetben a is konfigurálnia kell egy feltételt.</span><span class="sxs-lookup"><span data-stu-id="15cab-124">In addition toothese basic requirements, in many cases, you should also configure a condition.</span></span> <span data-ttu-id="15cab-125">Egy házirend akkor is megvalósítható, ha egy konfigurált feltételt, feltételek hello vezetői tényező a pontosabb beállításra hozzáférés tooyour alkalmazásokat is.</span><span class="sxs-lookup"><span data-stu-id="15cab-125">While a policy would also work without a configured condition, conditions are hello driving factor for fine-tuning access tooyour apps.</span></span>


![Felhőalkalmazások](./media/active-directory-conditional-access-best-practices/04.png)



### <a name="how-are-assignments-evaluated"></a><span data-ttu-id="15cab-127">Hogyan hozzárendelések értékeli ki a rendszer?</span><span class="sxs-lookup"><span data-stu-id="15cab-127">How are assignments evaluated?</span></span>

<span data-ttu-id="15cab-128">Minden hozzárendeléseket a rendszer logikailag **műveletet**.</span><span class="sxs-lookup"><span data-stu-id="15cab-128">All assignments are logically **ANDed**.</span></span> <span data-ttu-id="15cab-129">Ha egynél több hozzárendelés konfigurálva, egy házirend tootrigger vonatkozó összes hozzárendelést kell biztosítani.</span><span class="sxs-lookup"><span data-stu-id="15cab-129">If you have more than one assignment configured, tootrigger a policy, all assignments must be satisfied.</span></span>  

<span data-ttu-id="15cab-130">Ha módosítania kell a vállalati hálózaton kívülről érkezett tooall kapcsolatok olyan hely feltételt tooconfigure, ez által elvégezhető:</span><span class="sxs-lookup"><span data-stu-id="15cab-130">If you need tooconfigure a location condition that applies tooall connections made from outside your organization's network, you can accomplish this by:</span></span>

- <span data-ttu-id="15cab-131">Beleértve **az összes hely**</span><span class="sxs-lookup"><span data-stu-id="15cab-131">Including **All locations**</span></span>
- <span data-ttu-id="15cab-132">Kivéve **a megbízható IP-címek**</span><span class="sxs-lookup"><span data-stu-id="15cab-132">Excluding **All trusted IPs**</span></span>

### <a name="what-happens-if-you-have-policies-in-hello-azure-classic-portal-and-azure-portal-configured"></a><span data-ttu-id="15cab-133">Mi történik, ha a klasszikus Azure portálon hello házirendek és konfigurált Azure-portálon?</span><span class="sxs-lookup"><span data-stu-id="15cab-133">What happens if you have policies in hello Azure classic portal and Azure portal configured?</span></span>  
<span data-ttu-id="15cab-134">Mindkét házirend Azure Active Directory érvényesíti, és hello felhasználó élvezheti a hozzáférést, csak akkor, ha összes követelmény teljesülését.</span><span class="sxs-lookup"><span data-stu-id="15cab-134">Both policies are enforced by Azure Active Directory and hello user gets access only when all requirements are met.</span></span>

### <a name="what-happens-if-you-have-policies-in-hello-intune-silverlight-portal-and-hello-azure-portal"></a><span data-ttu-id="15cab-135">Mi történik, ha a házirendek segítségével a hello Intune Silverlight portal és hello Azure-portálon?</span><span class="sxs-lookup"><span data-stu-id="15cab-135">What happens if you have policies in hello Intune Silverlight portal and hello Azure Portal?</span></span>
<span data-ttu-id="15cab-136">Mindkét házirend Azure Active Directory érvényesíti, és hello felhasználó élvezheti a hozzáférést, csak akkor, ha összes követelmény teljesülését.</span><span class="sxs-lookup"><span data-stu-id="15cab-136">Both policies are enforced by Azure Active Directory and hello user gets access only when all requirements are met.</span></span>

### <a name="what-happens-if-i-have-multiple-policies-for-hello-same-user-configured"></a><span data-ttu-id="15cab-137">Mi történik, ha ugyanazon felhasználó konfigurált hello több házirendeket?</span><span class="sxs-lookup"><span data-stu-id="15cab-137">What happens if I have multiple policies for hello same user configured?</span></span>  
<span data-ttu-id="15cab-138">Minden bejelentkezés az Azure Active Directory kiértékeli az összes házirend, és biztosítja, hogy minden követelmények teljesülnek-e megadott hozzáférési toohello felhasználói előtt.</span><span class="sxs-lookup"><span data-stu-id="15cab-138">For every sign-in, Azure Active Directory evaluates all policies and ensures that all requirements are met before granted access toohello user.</span></span>


### <a name="does-conditional-access-work-with-exchange-activesync"></a><span data-ttu-id="15cab-139">Feltételes hozzáférés az Exchange ActiveSync szolgáltatással működik?</span><span class="sxs-lookup"><span data-stu-id="15cab-139">Does conditional access work with Exchange ActiveSync?</span></span>

<span data-ttu-id="15cab-140">Igen, az Exchange ActiveSync is használhatja a feltételes hozzáférési házirendben.</span><span class="sxs-lookup"><span data-stu-id="15cab-140">Yes, you can use Exchange ActiveSync in a conditional access policy.</span></span>


## <a name="what-you-should-avoid-doing"></a><span data-ttu-id="15cab-141">Mi kerülendő ezzel</span><span class="sxs-lookup"><span data-stu-id="15cab-141">What you should avoid doing</span></span>

<span data-ttu-id="15cab-142">hello feltételes hozzáférés keretrendszer egy nagyszerű konfigurációs rugalmasságot biztosít.</span><span class="sxs-lookup"><span data-stu-id="15cab-142">hello conditional access framework provides you with a great configuration flexibility.</span></span> <span data-ttu-id="15cab-143">Azonban rugalmas lehetőségeket biztosítanak azt is jelenti, hogy alaposan tekintse át minden konfigurációs házirend előzetes tooreleasing azt tooavoid nemkívánatos eredmények.</span><span class="sxs-lookup"><span data-stu-id="15cab-143">However, great flexibility  also means that you should carefully review each configuration policy prior tooreleasing it tooavoid undesirable results.</span></span> <span data-ttu-id="15cab-144">Ebben a környezetben, mint a teljes érintő különös figyelmet tooassignments kiválasztásánál kell **minden felhasználók / csoportok / felhőalapú alkalmazásokba**.</span><span class="sxs-lookup"><span data-stu-id="15cab-144">In this context, you should pay special attention tooassignments affecting complete sets such as **all users / groups / cloud apps**.</span></span>

<span data-ttu-id="15cab-145">A környezetben a következő konfigurációk hello kerülje el:</span><span class="sxs-lookup"><span data-stu-id="15cab-145">In your environment, you should avoid hello following configurations:</span></span>


<span data-ttu-id="15cab-146">**Az összes felhasználó számára az összes felhőalapú alkalmazásokat:**</span><span class="sxs-lookup"><span data-stu-id="15cab-146">**For all users, all cloud apps:**</span></span>

- <span data-ttu-id="15cab-147">**Letiltja a hozzáférést,** -ebben a konfigurációban a teljes szervezet, amely nincs mindenképpen célszerű blokkolja.</span><span class="sxs-lookup"><span data-stu-id="15cab-147">**Block access** - This configuration blocks your entire organization, which is definitely not a good idea.</span></span>

- <span data-ttu-id="15cab-148">**Megfelelő eszközökre szükséges** – a felhasználók számára, amely nem regisztrálták az eszközeiket, mégis a házirend nem engedélyezi az összes eléréséhez, beleértve a hozzáférési toohello Intune portál.</span><span class="sxs-lookup"><span data-stu-id="15cab-148">**Require compliant device** - For users that don't have enrolled their devices yet, this policy blocks all access including access toohello Intune portal.</span></span> <span data-ttu-id="15cab-149">Ha Ön rendszergazda a regisztrált eszköz nélküli, akkor ez a házirend letiltja a hello Azure portál toochange hello házirendbe vissza fog.</span><span class="sxs-lookup"><span data-stu-id="15cab-149">If you are an administrator without an enrolled device, this policy blocks you from getting back into hello Azure portal toochange hello policy.</span></span>

- <span data-ttu-id="15cab-150">**Megkövetelje a tartományhoz való csatlakozást** – Ez a házirend-blokk hozzáférést is hello lehetséges tooblock hozzáféréssel rendelkezik az összes felhasználó számára a szervezet Ha még nem rendelkezik a tartományhoz csatlakoztatott eszközön.</span><span class="sxs-lookup"><span data-stu-id="15cab-150">**Require domain join** - This policy block access has also hello potential tooblock access for all users in your organization if you don't have a domain-joined device yet.</span></span>


<span data-ttu-id="15cab-151">**Az összes felhasználó számára az összes felhőalapú alkalmazások, az összes eszközplatformra:**</span><span class="sxs-lookup"><span data-stu-id="15cab-151">**For all users, all cloud apps, all device platforms:**</span></span>

- <span data-ttu-id="15cab-152">**Letiltja a hozzáférést,** -ebben a konfigurációban a teljes szervezet, amely nincs mindenképpen célszerű blokkolja.</span><span class="sxs-lookup"><span data-stu-id="15cab-152">**Block access** - This configuration blocks your entire organization, which is definitely not a good idea.</span></span>


## <a name="common-scenarios"></a><span data-ttu-id="15cab-153">Gyakori forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="15cab-153">Common scenarios</span></span>

### <a name="requiring-multi-factor-authentication-for-apps"></a><span data-ttu-id="15cab-154">Többtényezős hitelesítés megkövetelése az alkalmazások</span><span class="sxs-lookup"><span data-stu-id="15cab-154">Requiring multi-factor authentication for apps</span></span>

<span data-ttu-id="15cab-155">Sok környezetben a magasabb szintű védelem hello mint mások igénylő alkalmazások rendelkeznek.</span><span class="sxs-lookup"><span data-stu-id="15cab-155">Many environments have apps requiring a higher level of protection than hello others.</span></span>
<span data-ttu-id="15cab-156">Ez helyzet, például hello access toosensitive adatok rendelkező alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="15cab-156">This is, for example, hello case for apps that have access toosensitive data.</span></span>
<span data-ttu-id="15cab-157">Ha azt szeretné, tooadd toothese alkalmazások védelmi réteget, konfigurálhatja egy feltételes hozzáférési szabályzatot, amely többtényezős hitelesítést igényel, amikor a felhasználók elérik-e ezeket az alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="15cab-157">If you want tooadd another layer of protection toothese apps, you can configure a conditional access policy that requires multi-factor authentication when users are accessing these apps.</span></span>


### <a name="requiring-multi-factor-authentication-for-access-from-networks-that-are-not-trusted"></a><span data-ttu-id="15cab-158">Többtényezős hitelesítés megkövetelése a nem megbízható hálózatokhoz való hozzáférést</span><span class="sxs-lookup"><span data-stu-id="15cab-158">Requiring multi-factor authentication for access from networks that are not trusted</span></span>

<span data-ttu-id="15cab-159">Ebben a forgatókönyvben nem hasonló toohello előző helyzethez, mert a multi-factor authentication követelmény hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="15cab-159">This scenario is similar toohello previous scenario because it adds a requirement for multi-factor authentication.</span></span>
<span data-ttu-id="15cab-160">Hello fő különbség azonban ezt a követelményt hello feltételét.</span><span class="sxs-lookup"><span data-stu-id="15cab-160">However, hello main difference is hello condition for this requirement.</span></span>  
<span data-ttu-id="15cab-161">Hello célja az előző példában hello access toosensitve adatok az alkalmazások állapotában hello ebben a forgatókönyvben elsősorban megbízható helyeken.</span><span class="sxs-lookup"><span data-stu-id="15cab-161">While hello focus of hello previous scenario was on apps with access toosensitve data, hello focus of this scenario is on trusted locations.</span></span>  
<span data-ttu-id="15cab-162">Ez azt jelenti lehetséges, hogy a multi-factor authentication követelmény az alkalmazások a felhasználó nem megbízható hálózaton keresztül illetéktelen.</span><span class="sxs-lookup"><span data-stu-id="15cab-162">In other words, you might have a requirement for multi-factor authentication if an app is accessed by a user from a network you don't trust.</span></span>


### <a name="only-trusted-devices-can-access-office-365-services"></a><span data-ttu-id="15cab-163">Csak a megbízható eszközök férhetnek hozzá az Office 365-szolgáltatásokhoz</span><span class="sxs-lookup"><span data-stu-id="15cab-163">Only trusted devices can access Office 365 services</span></span>

<span data-ttu-id="15cab-164">Ha Intune használ a környezetben, azonnal megkezdheti hello feltételes hozzáférési házirendek felülete a hello Azure-konzolon.</span><span class="sxs-lookup"><span data-stu-id="15cab-164">If you are using Intune in your environment, you can immediately start using hello conditional access policy interface in hello Azure console.</span></span>

<span data-ttu-id="15cab-165">Számos Intune használják az ügyfelek a feltételes hozzáférés tooensure, hogy csak a megbízható eszközök férhetnek hozzá az Office 365-szolgáltatásokhoz.</span><span class="sxs-lookup"><span data-stu-id="15cab-165">Many Intune customers are using conditional access tooensure that only trusted devices can access Office 365 services.</span></span> <span data-ttu-id="15cab-166">Ez azt jelenti, hogy a mobileszközök az Intune-nal beléptetett és megfelelőségi házirend követelményeknek, és a Windows rendszerű számítógépek illesztett tooan helyszíni tartományban.</span><span class="sxs-lookup"><span data-stu-id="15cab-166">This means that mobile devices are enrolled with Intune and meet compliance policy requirements, and that Windows PCs are joined tooan on-premises domain.</span></span> <span data-ttu-id="15cab-167">A kulcs fokozása, hogy nem rendelkezik tooset ugyanabban a házirendben hello hello Office 365-szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="15cab-167">A key improvement is that you do not have tooset hello same policy for each of hello Office 365 services.</span></span>  <span data-ttu-id="15cab-168">Ha egy új házirendet hoz létre, konfigurálja a hello Cloud apps tooinclude minden feltételes hozzáféréssel rendelkező tooprotect kívánja hello Office 365-alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="15cab-168">When you create a new policy, configure hello Cloud apps tooinclude each of hello O365 apps that you wish tooprotect with  with Conditional Access.</span></span>

## <a name="next-steps"></a><span data-ttu-id="15cab-169">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="15cab-169">Next steps</span></span>

<span data-ttu-id="15cab-170">Ha azt szeretné, hogyan tooconfigure egy feltételes hozzáférési szabályzatot: tooknow [Ismerkedés a feltételes hozzáférés az Azure Active Directoryban](active-directory-conditional-access-azure-portal-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="15cab-170">If you want tooknow how tooconfigure a conditional access policy, see [Get started with conditional access in Azure Active Directory](active-directory-conditional-access-azure-portal-get-started.md).</span></span>
