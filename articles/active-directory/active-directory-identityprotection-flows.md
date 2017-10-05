---
title: "Bejelentkezés során lép az Azure AD Identity Protection |} Microsoft Docs"
description: "A felhasználói élmény áttekintést nyújt, ha Identity Protection problémák elhárításáról vagy javítja a felhasználó, vagy ha a többtényezős hitelesítési házirend szükséges."
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
ms.openlocfilehash: e45936280b51fb2e54012a688fceddcc8dabe984
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="sign-in-experiences-with-azure-ad-identity-protection"></a><span data-ttu-id="4eb12-104">Az Azure AD Identity Protection bejelentkezési élmény</span><span class="sxs-lookup"><span data-stu-id="4eb12-104">Sign-in experiences with Azure AD Identity Protection</span></span>
<span data-ttu-id="4eb12-105">Az Azure Active Directory azonosító adatok védelmét a következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="4eb12-105">With Azure Active Directory Identity Protection, you can:</span></span>

* <span data-ttu-id="4eb12-106">a felhasználóknak a multi-factor authentication regisztrálása</span><span class="sxs-lookup"><span data-stu-id="4eb12-106">require users to register for multi-factor authentication</span></span>
* <span data-ttu-id="4eb12-107">kockázatos bejelentkezéseket és a sérült biztonságú felhasználók kezelése</span><span class="sxs-lookup"><span data-stu-id="4eb12-107">handle risky sign-ins and compromised users</span></span>

<span data-ttu-id="4eb12-108">A válasz a rendszer ezeket a problémákat a felhasználói bejelentkezés során tapasztal élmény hatással van, mert csak közvetlenül aláírás-a felhasználónév megadásával, és a jelszó nem lehet többé.</span><span class="sxs-lookup"><span data-stu-id="4eb12-108">The response of the system to these issues has an impact on a user's sign-in experience because just directly signing-in by providing a user name and a password won't be possible anymore.</span></span> <span data-ttu-id="4eb12-109">További lépések szükségesek a felhasználók biztonságosan beolvasandó business programba.</span><span class="sxs-lookup"><span data-stu-id="4eb12-109">Additional steps are required to get a user safely back into business.</span></span>

<span data-ttu-id="4eb12-110">Ez a témakör áttekintést nyújt a felhasználói bejelentkezési felhasználói felület minden olyan esetben, amik akkor léphetnek fel.</span><span class="sxs-lookup"><span data-stu-id="4eb12-110">This topic gives you an overview of a user's sign-in experience for all cases that can occur.</span></span>

<span data-ttu-id="4eb12-111">**Többtényezős hitelesítés**</span><span class="sxs-lookup"><span data-stu-id="4eb12-111">**Multi-factor authentication**</span></span>

* <span data-ttu-id="4eb12-112">A multi-factor authentication regisztráció</span><span class="sxs-lookup"><span data-stu-id="4eb12-112">Multi-factor authentication registration</span></span>

<span data-ttu-id="4eb12-113">**Bejelentkezés veszélyben**</span><span class="sxs-lookup"><span data-stu-id="4eb12-113">**Sign-in at risk**</span></span>

* <span data-ttu-id="4eb12-114">Kockázatos bejelentkezési helyreállítási</span><span class="sxs-lookup"><span data-stu-id="4eb12-114">Risky sign-in recovery</span></span>
* <span data-ttu-id="4eb12-115">Kockázatos bejelentkezés letiltva</span><span class="sxs-lookup"><span data-stu-id="4eb12-115">Risky sign-in blocked</span></span>
* <span data-ttu-id="4eb12-116">A multi-factor authentication regisztráció egy kockázatos bejelentkezés során</span><span class="sxs-lookup"><span data-stu-id="4eb12-116">Multi-factor authentication registration during a risky sign-in</span></span>

<span data-ttu-id="4eb12-117">**Felhasználói veszélyben**</span><span class="sxs-lookup"><span data-stu-id="4eb12-117">**User at risk**</span></span>

* <span data-ttu-id="4eb12-118">Sérült biztonságú fiók helyreállítási</span><span class="sxs-lookup"><span data-stu-id="4eb12-118">Compromised account recovery</span></span>
* <span data-ttu-id="4eb12-119">Sérült biztonságú fiók blokkolva van</span><span class="sxs-lookup"><span data-stu-id="4eb12-119">Compromised account blocked</span></span>

## <a name="multi-factor-authentication-registration"></a><span data-ttu-id="4eb12-120">A multi-factor authentication regisztráció</span><span class="sxs-lookup"><span data-stu-id="4eb12-120">Multi-factor authentication registration</span></span>
<span data-ttu-id="4eb12-121">A legjobb felhasználói élmény is, a sérült biztonságú fiók helyreállítási folyamata és a kockázatos bejelentkezési folyamata, akkor, ha a felhasználó önállóan helyre tudja állítani.</span><span class="sxs-lookup"><span data-stu-id="4eb12-121">The best user experience for both, the compromised account recovery flow and the risky sign-in flow, is when the user can self-recover.</span></span> <span data-ttu-id="4eb12-122">Ha a felhasználók a multi-factor authentication van regisztrálva, már rendelkeznek használható jelentette biztonsági kihívásokkal átadni a fiókjához társított telefonszám.</span><span class="sxs-lookup"><span data-stu-id="4eb12-122">If users are registered for multi-factor authentication, they already have a phone number associated with their account that can be used to pass security challenges.</span></span> <span data-ttu-id="4eb12-123">A fiók biztonsága sérülésétől helyreállítása nincs súgó ügyfélszolgálat vagy a rendszergazdai beavatkozás szükséges.</span><span class="sxs-lookup"><span data-stu-id="4eb12-123">No help desk or administrator involvement is needed to recover from account compromise.</span></span> <span data-ttu-id="4eb12-124">Így az rendelkezik kifejezetten ajánljuk, hogy a felhasználók a többtényezős hitelesítés regisztrált.</span><span class="sxs-lookup"><span data-stu-id="4eb12-124">Thus, it’s highly recommended to get your users registered for multi-factor authentication.</span></span> 

<span data-ttu-id="4eb12-125">A rendszergazdák a következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="4eb12-125">Administrators can:</span></span>

* <span data-ttu-id="4eb12-126">Állítsa be a további biztonsági ellenőrzési felhasználóknak házirend beállítása.</span><span class="sxs-lookup"><span data-stu-id="4eb12-126">set a policy that requires users to set up their accounts for additional security verification.</span></span> 
* <span data-ttu-id="4eb12-127">engedélyezi a multi-factor authentication regisztráció kihagyása akár 30 napig, abban az esetben, ha szeretnék lehetőséget nyújt a felhasználóknak a türelmi időszak, mielőtt regisztrálná.</span><span class="sxs-lookup"><span data-stu-id="4eb12-127">allow skipping multi-factor authentication registration for up to 30 days, in case they want to give users a grace period before registering.</span></span>

<span data-ttu-id="4eb12-128">**A multi-factor Authentication hitelesítés regisztrációs rendelkezik három lépést:**</span><span class="sxs-lookup"><span data-stu-id="4eb12-128">**The multi-factor authentication registration has three steps:**</span></span>

1. <span data-ttu-id="4eb12-129">Az első lépésben a felhasználó élvezheti a követelmény a multi-factor Authentication fiók értesítést.</span><span class="sxs-lookup"><span data-stu-id="4eb12-129">In the first step, the user gets a notification about the requirement to set the account up for multi-factor authentication.</span></span> 
   
    <span data-ttu-id="4eb12-130">![Szervizelés](./media/active-directory-identityprotection-flows/140.png "szervizelés")</span><span class="sxs-lookup"><span data-stu-id="4eb12-130">![Remediation](./media/active-directory-identityprotection-flows/140.png "Remediation")</span></span>
2. <span data-ttu-id="4eb12-131">Többtényezős hitelesítés beállításához, hogy a rendszer tudja, hogyan szeretné kapcsolódni kell.</span><span class="sxs-lookup"><span data-stu-id="4eb12-131">To set multi-factor authentication up, you need to let the system know how you want to be contacted.</span></span>
   
    <span data-ttu-id="4eb12-132">![Szervizelés](./media/active-directory-identityprotection-flows/141.png "szervizelés")</span><span class="sxs-lookup"><span data-stu-id="4eb12-132">![Remediation](./media/active-directory-identityprotection-flows/141.png "Remediation")</span></span>
3. <span data-ttu-id="4eb12-133">A rendszer elküldi a könnyű, és meg kell válaszolni.</span><span class="sxs-lookup"><span data-stu-id="4eb12-133">The system submits a challenge to you and you need to respond.</span></span>
   
    <span data-ttu-id="4eb12-134">![Szervizelés](./media/active-directory-identityprotection-flows/142.png "szervizelés")</span><span class="sxs-lookup"><span data-stu-id="4eb12-134">![Remediation](./media/active-directory-identityprotection-flows/142.png "Remediation")</span></span>

## <a name="risky-sign-in-recovery"></a><span data-ttu-id="4eb12-135">Kockázatos bejelentkezési helyreállítási</span><span class="sxs-lookup"><span data-stu-id="4eb12-135">Risky sign-in recovery</span></span>
<span data-ttu-id="4eb12-136">Amikor egy rendszergazda úgy konfigurálta a kockázatok bejelentkezési házirend, az érintett felhasználó értesítést kap próbálnak bejelentkezni.</span><span class="sxs-lookup"><span data-stu-id="4eb12-136">When an administrator has configured a policy for sign-in risks, the affected users are notified when they try to sign-in.</span></span> 

<span data-ttu-id="4eb12-137">**A kockázatos bejelentkezési folyamata van két lépésből áll:**</span><span class="sxs-lookup"><span data-stu-id="4eb12-137">**The risky sign-in flow has two steps:**</span></span> 

1. <span data-ttu-id="4eb12-138">A felhasználó tájékoztatják, hogy valami szokatlan rendszer észlelte a bejelentkezéssel kapcsolatban, például egy új helyről, eszközről vagy alkalmazásból jelentkezik be.</span><span class="sxs-lookup"><span data-stu-id="4eb12-138">The user is informed that something unusual was detected about their sign-in, such as signing in from a new location, device, or app.</span></span> 
   
    <span data-ttu-id="4eb12-139">![Szervizelés](./media/active-directory-identityprotection-flows/120.png "szervizelés")</span><span class="sxs-lookup"><span data-stu-id="4eb12-139">![Remediation](./media/active-directory-identityprotection-flows/120.png "Remediation")</span></span>
2. <span data-ttu-id="4eb12-140">A felhasználó kell igazolnia az identitását egy biztonsági kérdéssel megoldása által.</span><span class="sxs-lookup"><span data-stu-id="4eb12-140">The user is required to prove their identity by solving a security challenge.</span></span> <span data-ttu-id="4eb12-141">Ha a felhasználó a multi-factor authentication regisztrálva van szükségük round-trip telefonszámukat biztonsági kódot.</span><span class="sxs-lookup"><span data-stu-id="4eb12-141">If the user is registered for multi-factor authentication they need to round-trip a security code to their phone number.</span></span> <span data-ttu-id="4eb12-142">Mivel ez csak egy kockázatos bejelentkezési és nem sérült biztonságú fiók, a felhasználónak kell-e a nem a folyamatot a jelszó módosítására.</span><span class="sxs-lookup"><span data-stu-id="4eb12-142">Since this is a just a risky sign in and not a compromised account, the user won’t have to change the password in this flow.</span></span> 
   
    <span data-ttu-id="4eb12-143">![Szervizelés](./media/active-directory-identityprotection-flows/121.png "szervizelés")</span><span class="sxs-lookup"><span data-stu-id="4eb12-143">![Remediation](./media/active-directory-identityprotection-flows/121.png "Remediation")</span></span>

## <a name="risky-sign-in-blocked"></a><span data-ttu-id="4eb12-144">Kockázatos bejelentkezés letiltva</span><span class="sxs-lookup"><span data-stu-id="4eb12-144">Risky sign-in blocked</span></span>
<span data-ttu-id="4eb12-145">A rendszergazdák is beállíthatja a kockázat bejelentkezési házirend beállítása a felhasználók blokkolása után bejelentkezhet attól függően, hogy a kockázati szintjét.</span><span class="sxs-lookup"><span data-stu-id="4eb12-145">Administrators can also choose to set a Sign-In Risk policy to block users upon sign-in depending on the risk level.</span></span> <span data-ttu-id="4eb12-146">Ahhoz, hogy feloldja, a végfelhasználók kapcsolatba kell lépnie egy rendszergazda vagy a help ügyfélszolgálati, vagy próbálnak jelentkezik be egy ismert helyre vagy egy eszközt.</span><span class="sxs-lookup"><span data-stu-id="4eb12-146">To get unblocked, end users must contact an administrator or help desk, or they can try signing in from a familiar location or device.</span></span> <span data-ttu-id="4eb12-147">A multi-factor authentication megoldása által önálló helyreállítása lehetőség nem érhető el ebben az esetben.</span><span class="sxs-lookup"><span data-stu-id="4eb12-147">Self-recovering by solving multi-factor authentication is not an option in this case.</span></span>

<span data-ttu-id="4eb12-148">![Szervizelés](./media/active-directory-identityprotection-flows/200.png "szervizelés")</span><span class="sxs-lookup"><span data-stu-id="4eb12-148">![Remediation](./media/active-directory-identityprotection-flows/200.png "Remediation")</span></span>

## <a name="compromised-account-recovery"></a><span data-ttu-id="4eb12-149">Sérült biztonságú fiók helyreállítási</span><span class="sxs-lookup"><span data-stu-id="4eb12-149">Compromised account recovery</span></span>
<span data-ttu-id="4eb12-150">Ha egy felhasználó kockázat biztonsági házirendet konfiguráltak, felhasználók, akik megfelelnek a felhasználó kockáztatja a házirendben megadott szint (és ezért feltételezik sérült) haladjon végig a felhasználó biztonsági sérülés helyreállítási folyamata, mielőtt azok is bejelentkezhet.</span><span class="sxs-lookup"><span data-stu-id="4eb12-150">When a user risk security policy has been configured, users who meet the user risk level specified in the policy (and are therefore assumed compromised) must go through the user compromise recovery flow before they can sign-in.</span></span> 

<span data-ttu-id="4eb12-151">**A felhasználó biztonsági sérülés helyreállítási folyamat három lépést rendelkezik:**</span><span class="sxs-lookup"><span data-stu-id="4eb12-151">**The user compromise recovery flow has three steps:**</span></span>

1. <span data-ttu-id="4eb12-152">A felhasználót, hogy a fiók biztonsági kockázatnak miatt gyanús tevékenységet vagy hitelesítő adatok szivárgását tájékoztatást kapjon.</span><span class="sxs-lookup"><span data-stu-id="4eb12-152">The user is informed that their account security is at risk because of suspicious activity or leaked credentials.</span></span>
   
    <span data-ttu-id="4eb12-153">![Szervizelés](./media/active-directory-identityprotection-flows/101.png "szervizelés")</span><span class="sxs-lookup"><span data-stu-id="4eb12-153">![Remediation](./media/active-directory-identityprotection-flows/101.png "Remediation")</span></span>
2. <span data-ttu-id="4eb12-154">A felhasználó kell igazolnia az identitását egy biztonsági kérdéssel megoldása által.</span><span class="sxs-lookup"><span data-stu-id="4eb12-154">The user is required to prove their identity by solving a security challenge.</span></span> <span data-ttu-id="4eb12-155">Ha a felhasználó a multi-factor authentication regisztrálva azok önállóan helyreállítani feltörésének.</span><span class="sxs-lookup"><span data-stu-id="4eb12-155">If the user is registered for multi-factor authentication they can self-recover from being compromised.</span></span> <span data-ttu-id="4eb12-156">Akkor kell round-trip telefonszámukat biztonsági kódot.</span><span class="sxs-lookup"><span data-stu-id="4eb12-156">They will need to round-trip a security code to their phone number.</span></span> 
   
   <span data-ttu-id="4eb12-157">![Szervizelés](./media/active-directory-identityprotection-flows/110.png "szervizelés")</span><span class="sxs-lookup"><span data-stu-id="4eb12-157">![Remediation](./media/active-directory-identityprotection-flows/110.png "Remediation")</span></span>
3. <span data-ttu-id="4eb12-158">Végezetül a felhasználónak meg kell a jelszó módosítására, mivel valaki más is hozzáférhetett a fiókjához.</span><span class="sxs-lookup"><span data-stu-id="4eb12-158">Finally, the user is forced to change their password since someone else may have had access to their account.</span></span> 
   <span data-ttu-id="4eb12-159">A felhasználói felület képernyőfelvételek alatt van.</span><span class="sxs-lookup"><span data-stu-id="4eb12-159">Screenshots of this experience are below.</span></span>
   
   <span data-ttu-id="4eb12-160">![Szervizelés](./media/active-directory-identityprotection-flows/111.png "szervizelés")</span><span class="sxs-lookup"><span data-stu-id="4eb12-160">![Remediation](./media/active-directory-identityprotection-flows/111.png "Remediation")</span></span>

## <a name="compromised-account-blocked"></a><span data-ttu-id="4eb12-161">Sérült biztonságú fiók blokkolva van</span><span class="sxs-lookup"><span data-stu-id="4eb12-161">Compromised account blocked</span></span>
<span data-ttu-id="4eb12-162">Ahhoz, hogy a felhasználó által feloldva felhasználói kockázat biztonsági házirend letiltott, a felhasználónak kell forduljon a rendszergazdához vagy a segélyszolgálathoz.</span><span class="sxs-lookup"><span data-stu-id="4eb12-162">To get a user that was blocked by a user risk security policy unblocked, the user must contact an administrator or help desk.</span></span> <span data-ttu-id="4eb12-163">A multi-factor authentication megoldása által önálló helyreállítása lehetőség nem érhető el ebben az esetben.</span><span class="sxs-lookup"><span data-stu-id="4eb12-163">Self-recovering by solving multi-factor authentication is not an option in this case.</span></span>

<span data-ttu-id="4eb12-164">![Szervizelés](./media/active-directory-identityprotection-flows/104.png "szervizelés")</span><span class="sxs-lookup"><span data-stu-id="4eb12-164">![Remediation](./media/active-directory-identityprotection-flows/104.png "Remediation")</span></span>

## <a name="reset-password"></a><span data-ttu-id="4eb12-165">Új jelszó létrehozása</span><span class="sxs-lookup"><span data-stu-id="4eb12-165">Reset password</span></span>
<span data-ttu-id="4eb12-166">Sérült biztonságú felhasználók nincs hozzáférése a bejelentkezés, ha a rendszergazda egy ideiglenes jelszót hozhat létre a számukra.</span><span class="sxs-lookup"><span data-stu-id="4eb12-166">If compromised users are blocked from signing in, an administrator can generate a temporary password for them.</span></span> <span data-ttu-id="4eb12-167">A felhasználók módosíthatják a jelszavukat a következő bejelentkezés során lesz.</span><span class="sxs-lookup"><span data-stu-id="4eb12-167">The users will have to change their password during a next sign-in.</span></span>

<span data-ttu-id="4eb12-168">![Szervizelés](./media/active-directory-identityprotection-flows/160.png "szervizelés")</span><span class="sxs-lookup"><span data-stu-id="4eb12-168">![Remediation](./media/active-directory-identityprotection-flows/160.png "Remediation")</span></span>

## <a name="see-also"></a><span data-ttu-id="4eb12-169">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="4eb12-169">See also</span></span>
* [<span data-ttu-id="4eb12-170">Az Azure Active Directory azonosító adatok védelmét</span><span class="sxs-lookup"><span data-stu-id="4eb12-170">Azure Active Directory Identity Protection</span></span>](active-directory-identityprotection.md) 

