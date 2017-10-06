---
title: "aaaSign az Azure AD Identity Protection lép |} Microsoft Docs"
description: "Hello felhasználói élmény áttekintést nyújt, ha Identity Protection problémák elhárításáról vagy javítja a felhasználó, vagy ha egy házirend többtényezős hitelesítés szükséges."
services: active-directory
keywords: "az Azure active directory azonosító adatok védelmét, a cloud app discovery, alkalmazások, biztonság, kockázat, kockázati szint, biztonsági rés, biztonsági házirend kezelése"
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: de5bf637-75a7-4104-b6d8-03686372a319
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: fbdca5b86ed93d0a2f2b6df1dd0150da9c0c85c0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="sign-in-experiences-with-azure-ad-identity-protection"></a><span data-ttu-id="7ab8f-104">Az Azure AD Identity Protection bejelentkezési élmény</span><span class="sxs-lookup"><span data-stu-id="7ab8f-104">Sign-in experiences with Azure AD Identity Protection</span></span>
<span data-ttu-id="7ab8f-105">Az Azure Active Directory azonosító adatok védelmét a következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="7ab8f-105">With Azure Active Directory Identity Protection, you can:</span></span>

* <span data-ttu-id="7ab8f-106">felhasználók tooregister a többtényezős hitelesítés megkövetelése</span><span class="sxs-lookup"><span data-stu-id="7ab8f-106">require users tooregister for multi-factor authentication</span></span>
* <span data-ttu-id="7ab8f-107">kockázatos bejelentkezéseket és a sérült biztonságú felhasználók kezelése</span><span class="sxs-lookup"><span data-stu-id="7ab8f-107">handle risky sign-ins and compromised users</span></span>

<span data-ttu-id="7ab8f-108">hello válasz hello rendszer toothese problémák hatással van a felhasználói bejelentkezés során tapasztal élmény mivel most közvetlen aláírás-a felhasználónév és jelszó megadásával nem lesznek lehetséges többé.</span><span class="sxs-lookup"><span data-stu-id="7ab8f-108">hello response of hello system toothese issues has an impact on a user's sign-in experience because just directly signing-in by providing a user name and a password won't be possible anymore.</span></span> <span data-ttu-id="7ab8f-109">További lépések végrehajtására szükség a felhasználók biztonságosan vissza az üzleti tooget.</span><span class="sxs-lookup"><span data-stu-id="7ab8f-109">Additional steps are required tooget a user safely back into business.</span></span>

<span data-ttu-id="7ab8f-110">Ez a témakör áttekintést nyújt a felhasználói bejelentkezési felhasználói felület minden olyan esetben, amik akkor léphetnek fel.</span><span class="sxs-lookup"><span data-stu-id="7ab8f-110">This topic gives you an overview of a user's sign-in experience for all cases that can occur.</span></span>

<span data-ttu-id="7ab8f-111">**Többtényezős hitelesítés**</span><span class="sxs-lookup"><span data-stu-id="7ab8f-111">**Multi-factor authentication**</span></span>

* <span data-ttu-id="7ab8f-112">A multi-factor authentication regisztráció</span><span class="sxs-lookup"><span data-stu-id="7ab8f-112">Multi-factor authentication registration</span></span>

<span data-ttu-id="7ab8f-113">**Bejelentkezés veszélyben**</span><span class="sxs-lookup"><span data-stu-id="7ab8f-113">**Sign-in at risk**</span></span>

* <span data-ttu-id="7ab8f-114">Kockázatos bejelentkezési helyreállítási</span><span class="sxs-lookup"><span data-stu-id="7ab8f-114">Risky sign-in recovery</span></span>
* <span data-ttu-id="7ab8f-115">Kockázatos bejelentkezés letiltva</span><span class="sxs-lookup"><span data-stu-id="7ab8f-115">Risky sign-in blocked</span></span>
* <span data-ttu-id="7ab8f-116">A multi-factor authentication regisztráció egy kockázatos bejelentkezés során</span><span class="sxs-lookup"><span data-stu-id="7ab8f-116">Multi-factor authentication registration during a risky sign-in</span></span>

<span data-ttu-id="7ab8f-117">**Felhasználói veszélyben**</span><span class="sxs-lookup"><span data-stu-id="7ab8f-117">**User at risk**</span></span>

* <span data-ttu-id="7ab8f-118">Sérült biztonságú fiók helyreállítási</span><span class="sxs-lookup"><span data-stu-id="7ab8f-118">Compromised account recovery</span></span>
* <span data-ttu-id="7ab8f-119">Sérült biztonságú fiók blokkolva van</span><span class="sxs-lookup"><span data-stu-id="7ab8f-119">Compromised account blocked</span></span>

## <a name="multi-factor-authentication-registration"></a><span data-ttu-id="7ab8f-120">A multi-factor authentication regisztráció</span><span class="sxs-lookup"><span data-stu-id="7ab8f-120">Multi-factor authentication registration</span></span>
<span data-ttu-id="7ab8f-121">hello mind a lehető legjobb felhasználói élmény, sérült biztonságú fiók helyreállítási folyamata hello hello kockázatos bejelentkezési folyamata, amikor hello felhasználói önálló helyre tudja állítani.</span><span class="sxs-lookup"><span data-stu-id="7ab8f-121">hello best user experience for both, hello compromised account recovery flow and hello risky sign-in flow, is when hello user can self-recover.</span></span> <span data-ttu-id="7ab8f-122">Ha a felhasználók a multi-factor authentication van regisztrálva, már rendelkeznek, amelyek lehetnek használt toopass jelentette biztonsági kihívásokkal a fiókjához társított telefonszám.</span><span class="sxs-lookup"><span data-stu-id="7ab8f-122">If users are registered for multi-factor authentication, they already have a phone number associated with their account that can be used toopass security challenges.</span></span> <span data-ttu-id="7ab8f-123">Nincs súgó ügyfélszolgálat vagy a rendszergazdai beavatkozás szükséges toorecover a fiók biztonsága sérülésétől.</span><span class="sxs-lookup"><span data-stu-id="7ab8f-123">No help desk or administrator involvement is needed toorecover from account compromise.</span></span> <span data-ttu-id="7ab8f-124">Ebből kifolyólag magas javasoljuk tooget a felhasználók a többtényezős hitelesítés regisztrált.</span><span class="sxs-lookup"><span data-stu-id="7ab8f-124">Thus, it’s highly recommended tooget your users registered for multi-factor authentication.</span></span> 

<span data-ttu-id="7ab8f-125">A rendszergazdák a következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="7ab8f-125">Administrators can:</span></span>

* <span data-ttu-id="7ab8f-126">azok a fiókok létrehozása a felhasználók tooset további biztonsági ellenőrzést igénylő házirend beállítása.</span><span class="sxs-lookup"><span data-stu-id="7ab8f-126">set a policy that requires users tooset up their accounts for additional security verification.</span></span> 
* <span data-ttu-id="7ab8f-127">engedélyezi a mentést too30 nap, a multi-factor authentication regisztrálása kihagyása abban az esetben, ha szeretnék toogive a türelmi időszak, mielőtt regisztrálná.</span><span class="sxs-lookup"><span data-stu-id="7ab8f-127">allow skipping multi-factor authentication registration for up too30 days, in case they want toogive users a grace period before registering.</span></span>

<span data-ttu-id="7ab8f-128">**a multi-factor authentication regisztráció hello rendelkezik három lépést:**</span><span class="sxs-lookup"><span data-stu-id="7ab8f-128">**hello multi-factor authentication registration has three steps:**</span></span>

1. <span data-ttu-id="7ab8f-129">Hello első lépésként hello felhasználói hello követelmény tooset hello fiók az a multi-factor authentication szolgáltatáshoz értesítést kap.</span><span class="sxs-lookup"><span data-stu-id="7ab8f-129">In hello first step, hello user gets a notification about hello requirement tooset hello account up for multi-factor authentication.</span></span> 
   
    <span data-ttu-id="7ab8f-130">![Szervizelés](./media/active-directory-identityprotection-flows/140.png "szervizelés")</span><span class="sxs-lookup"><span data-stu-id="7ab8f-130">![Remediation](./media/active-directory-identityprotection-flows/140.png "Remediation")</span></span>
2. <span data-ttu-id="7ab8f-131">toolet hello rendszer tooset multi-factor authentication fel kell tudni kapcsolódni toobe módját.</span><span class="sxs-lookup"><span data-stu-id="7ab8f-131">tooset multi-factor authentication up, you need toolet hello system know how you want toobe contacted.</span></span>
   
    <span data-ttu-id="7ab8f-132">![Szervizelés](./media/active-directory-identityprotection-flows/141.png "szervizelés")</span><span class="sxs-lookup"><span data-stu-id="7ab8f-132">![Remediation](./media/active-directory-identityprotection-flows/141.png "Remediation")</span></span>
3. <span data-ttu-id="7ab8f-133">hello rendszer elküld egy ellenőrző tooyou és toorespond van szüksége.</span><span class="sxs-lookup"><span data-stu-id="7ab8f-133">hello system submits a challenge tooyou and you need toorespond.</span></span>
   
    <span data-ttu-id="7ab8f-134">![Szervizelés](./media/active-directory-identityprotection-flows/142.png "szervizelés")</span><span class="sxs-lookup"><span data-stu-id="7ab8f-134">![Remediation](./media/active-directory-identityprotection-flows/142.png "Remediation")</span></span>

## <a name="risky-sign-in-recovery"></a><span data-ttu-id="7ab8f-135">Kockázatos bejelentkezési helyreállítási</span><span class="sxs-lookup"><span data-stu-id="7ab8f-135">Risky sign-in recovery</span></span>
<span data-ttu-id="7ab8f-136">Amikor egy rendszergazda úgy konfigurálta a kockázatok bejelentkezési házirend, ha mégis megpróbálják toosign a hello érintett felhasználók értesítést kap.</span><span class="sxs-lookup"><span data-stu-id="7ab8f-136">When an administrator has configured a policy for sign-in risks, hello affected users are notified when they try toosign-in.</span></span> 

<span data-ttu-id="7ab8f-137">**hello kockázatos bejelentkezési folyamata két lépésből áll:**</span><span class="sxs-lookup"><span data-stu-id="7ab8f-137">**hello risky sign-in flow has two steps:**</span></span> 

1. <span data-ttu-id="7ab8f-138">hello felhasználói tájékoztatják, hogy valami szokatlan rendszer észlelte a bejelentkezéssel kapcsolatban, például egy új helyről, eszközről vagy alkalmazásból jelentkezik be.</span><span class="sxs-lookup"><span data-stu-id="7ab8f-138">hello user is informed that something unusual was detected about their sign-in, such as signing in from a new location, device, or app.</span></span> 
   
    <span data-ttu-id="7ab8f-139">![Szervizelés](./media/active-directory-identityprotection-flows/120.png "szervizelés")</span><span class="sxs-lookup"><span data-stu-id="7ab8f-139">![Remediation](./media/active-directory-identityprotection-flows/120.png "Remediation")</span></span>
2. <span data-ttu-id="7ab8f-140">hello felhasználó van szükség tooprove az identitásukat egy biztonsági kérdéssel megoldása.</span><span class="sxs-lookup"><span data-stu-id="7ab8f-140">hello user is required tooprove their identity by solving a security challenge.</span></span> <span data-ttu-id="7ab8f-141">Ha hello a felhasználó a multi-factor authentication regisztrálva van szükségük tooround-út egy biztonsági kódot tootheir telefonszámot.</span><span class="sxs-lookup"><span data-stu-id="7ab8f-141">If hello user is registered for multi-factor authentication they need tooround-trip a security code tootheir phone number.</span></span> <span data-ttu-id="7ab8f-142">Mivel ez csak egy kockázatos bejelentkezési és a sérült biztonságú fiók, hello felhasználó nem rendelkezik toochange hello jelszó a folyamatot.</span><span class="sxs-lookup"><span data-stu-id="7ab8f-142">Since this is a just a risky sign in and not a compromised account, hello user won’t have toochange hello password in this flow.</span></span> 
   
    <span data-ttu-id="7ab8f-143">![Szervizelés](./media/active-directory-identityprotection-flows/121.png "szervizelés")</span><span class="sxs-lookup"><span data-stu-id="7ab8f-143">![Remediation](./media/active-directory-identityprotection-flows/121.png "Remediation")</span></span>

## <a name="risky-sign-in-blocked"></a><span data-ttu-id="7ab8f-144">Kockázatos bejelentkezés letiltva</span><span class="sxs-lookup"><span data-stu-id="7ab8f-144">Risky sign-in blocked</span></span>
<span data-ttu-id="7ab8f-145">A rendszergazdák is kiválaszthatják tooset egy bejelentkezési kockázat házirend tooblock felhasználók után bejelentkezhet attól függően, hogy hello kockázati szintjét.</span><span class="sxs-lookup"><span data-stu-id="7ab8f-145">Administrators can also choose tooset a Sign-In Risk policy tooblock users upon sign-in depending on hello risk level.</span></span> <span data-ttu-id="7ab8f-146">tooget feloldva, a végfelhasználók kapcsolatba kell lépnie egy rendszergazda vagy a segélyszolgálat segítségét, vagy próbálnak jelentkezik be egy ismert helyre vagy egy eszközt.</span><span class="sxs-lookup"><span data-stu-id="7ab8f-146">tooget unblocked, end users must contact an administrator or help desk, or they can try signing in from a familiar location or device.</span></span> <span data-ttu-id="7ab8f-147">A multi-factor authentication megoldása által önálló helyreállítása lehetőség nem érhető el ebben az esetben.</span><span class="sxs-lookup"><span data-stu-id="7ab8f-147">Self-recovering by solving multi-factor authentication is not an option in this case.</span></span>

<span data-ttu-id="7ab8f-148">![Szervizelés](./media/active-directory-identityprotection-flows/200.png "szervizelés")</span><span class="sxs-lookup"><span data-stu-id="7ab8f-148">![Remediation](./media/active-directory-identityprotection-flows/200.png "Remediation")</span></span>

## <a name="compromised-account-recovery"></a><span data-ttu-id="7ab8f-149">Sérült biztonságú fiók helyreállítási</span><span class="sxs-lookup"><span data-stu-id="7ab8f-149">Compromised account recovery</span></span>
<span data-ttu-id="7ab8f-150">Amikor egy felhasználó kockázat biztonsági házirend konfigurálva van, a felhasználók, akik megfelelnek a hello felhasználói kockázati hello házirendben megadott szint (és ezért feltételezik sérült) hello felhasználói sérült biztonság esetén a helyreállítási folyamat keresztül kell haladnia, mielőtt azok is bejelentkezhet.</span><span class="sxs-lookup"><span data-stu-id="7ab8f-150">When a user risk security policy has been configured, users who meet hello user risk level specified in hello policy (and are therefore assumed compromised) must go through hello user compromise recovery flow before they can sign-in.</span></span> 

<span data-ttu-id="7ab8f-151">**hello felhasználói sérült biztonság esetén a helyreállítási folyamat három lépést rendelkezik:**</span><span class="sxs-lookup"><span data-stu-id="7ab8f-151">**hello user compromise recovery flow has three steps:**</span></span>

1. <span data-ttu-id="7ab8f-152">hello felhasználói tájékoztatják, hogy a fiók biztonsági kockázatnak miatt gyanús tevékenységet vagy hitelesítő adatok szivárgását.</span><span class="sxs-lookup"><span data-stu-id="7ab8f-152">hello user is informed that their account security is at risk because of suspicious activity or leaked credentials.</span></span>
   
    <span data-ttu-id="7ab8f-153">![Szervizelés](./media/active-directory-identityprotection-flows/101.png "szervizelés")</span><span class="sxs-lookup"><span data-stu-id="7ab8f-153">![Remediation](./media/active-directory-identityprotection-flows/101.png "Remediation")</span></span>
2. <span data-ttu-id="7ab8f-154">hello felhasználó van szükség tooprove az identitásukat egy biztonsági kérdéssel megoldása.</span><span class="sxs-lookup"><span data-stu-id="7ab8f-154">hello user is required tooprove their identity by solving a security challenge.</span></span> <span data-ttu-id="7ab8f-155">Ha hello felhasználó regisztrálva van a multi-factor authentication azok önállóan helyreállítani feltörésének.</span><span class="sxs-lookup"><span data-stu-id="7ab8f-155">If hello user is registered for multi-factor authentication they can self-recover from being compromised.</span></span> <span data-ttu-id="7ab8f-156">Egy biztonsági kódot tootheir telefonszámot tooround-út kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="7ab8f-156">They will need tooround-trip a security code tootheir phone number.</span></span> 
   
   <span data-ttu-id="7ab8f-157">![Szervizelés](./media/active-directory-identityprotection-flows/110.png "szervizelés")</span><span class="sxs-lookup"><span data-stu-id="7ab8f-157">![Remediation](./media/active-directory-identityprotection-flows/110.png "Remediation")</span></span>
3. <span data-ttu-id="7ab8f-158">Végezetül, a felhasználó hello is a jelszavukat kényszerített toochange mivel valaki más korábban hozzáférési tootheir fiók.</span><span class="sxs-lookup"><span data-stu-id="7ab8f-158">Finally, hello user is forced toochange their password since someone else may have had access tootheir account.</span></span> 
   <span data-ttu-id="7ab8f-159">A felhasználói felület képernyőfelvételek alatt van.</span><span class="sxs-lookup"><span data-stu-id="7ab8f-159">Screenshots of this experience are below.</span></span>
   
   <span data-ttu-id="7ab8f-160">![Szervizelés](./media/active-directory-identityprotection-flows/111.png "szervizelés")</span><span class="sxs-lookup"><span data-stu-id="7ab8f-160">![Remediation](./media/active-directory-identityprotection-flows/111.png "Remediation")</span></span>

## <a name="compromised-account-blocked"></a><span data-ttu-id="7ab8f-161">Sérült biztonságú fiók blokkolva van</span><span class="sxs-lookup"><span data-stu-id="7ab8f-161">Compromised account blocked</span></span>
<span data-ttu-id="7ab8f-162">tooget a felhasználó által feloldva felhasználói kockázat biztonsági házirend letiltott, hello felhasználónak kapcsolatba kell lépnie egy rendszergazda vagy a help ügyfélszolgálati.</span><span class="sxs-lookup"><span data-stu-id="7ab8f-162">tooget a user that was blocked by a user risk security policy unblocked, hello user must contact an administrator or help desk.</span></span> <span data-ttu-id="7ab8f-163">A multi-factor authentication megoldása által önálló helyreállítása lehetőség nem érhető el ebben az esetben.</span><span class="sxs-lookup"><span data-stu-id="7ab8f-163">Self-recovering by solving multi-factor authentication is not an option in this case.</span></span>

<span data-ttu-id="7ab8f-164">![Szervizelés](./media/active-directory-identityprotection-flows/104.png "szervizelés")</span><span class="sxs-lookup"><span data-stu-id="7ab8f-164">![Remediation](./media/active-directory-identityprotection-flows/104.png "Remediation")</span></span>

## <a name="reset-password"></a><span data-ttu-id="7ab8f-165">Új jelszó létrehozása</span><span class="sxs-lookup"><span data-stu-id="7ab8f-165">Reset password</span></span>
<span data-ttu-id="7ab8f-166">Sérült biztonságú felhasználók nincs hozzáférése a bejelentkezés, ha a rendszergazda egy ideiglenes jelszót hozhat létre a számukra.</span><span class="sxs-lookup"><span data-stu-id="7ab8f-166">If compromised users are blocked from signing in, an administrator can generate a temporary password for them.</span></span> <span data-ttu-id="7ab8f-167">hello felhasználónak lesz toochange a jelszavát a következő bejelentkezés során.</span><span class="sxs-lookup"><span data-stu-id="7ab8f-167">hello users will have toochange their password during a next sign-in.</span></span>

<span data-ttu-id="7ab8f-168">![Szervizelés](./media/active-directory-identityprotection-flows/160.png "szervizelés")</span><span class="sxs-lookup"><span data-stu-id="7ab8f-168">![Remediation](./media/active-directory-identityprotection-flows/160.png "Remediation")</span></span>

## <a name="see-also"></a><span data-ttu-id="7ab8f-169">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="7ab8f-169">See also</span></span>
* [<span data-ttu-id="7ab8f-170">Az Azure Active Directory azonosító adatok védelmét</span><span class="sxs-lookup"><span data-stu-id="7ab8f-170">Azure Active Directory Identity Protection</span></span>](active-directory-identityprotection.md) 

