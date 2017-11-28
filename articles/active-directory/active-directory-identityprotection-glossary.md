---
title: "Az Azure Active Directory-Identity Protection-szószedet |} Microsoft Docs"
description: "Az Azure Active Directory-Identity Protection-szószedet"
services: active-directory
keywords: "az Azure active directory azonosító adatok védelmét, a cloud app discovery, alkalmazások, biztonság, kockázati, kockázati szint, biztonsági rést, biztonsági házirend, szószedet kezelése"
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 833119a5-33d6-4482-adda-fa35218c72c3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 2cf64925cff9a78cf83532a1cfd231f7a1d98304
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="azure-active-directory-identity-protection-glossary"></a><span data-ttu-id="2cc43-104">Az Azure Active Directory-Identity Protection-szószedet</span><span class="sxs-lookup"><span data-stu-id="2cc43-104">Azure Active Directory Identity Protection Glossary</span></span>
### <a name="at-risk-user"></a><span data-ttu-id="2cc43-105">Fennáll a veszélye (felhasználó)</span><span class="sxs-lookup"><span data-stu-id="2cc43-105">At risk (User)</span></span>
<span data-ttu-id="2cc43-106">A felhasználó egy vagy több aktív kockázati események.</span><span class="sxs-lookup"><span data-stu-id="2cc43-106">A user with one or more active risk events.</span></span> 

### <a name="atypical-sign-in-location"></a><span data-ttu-id="2cc43-107">Bejelentkezés szokatlan bejelentkezési helye</span><span class="sxs-lookup"><span data-stu-id="2cc43-107">Atypical sign-in location</span></span>
<span data-ttu-id="2cc43-108">A bejelentkezés, amely nincs az adott felhasználó, hasonló felhasználók vagy a bérlő a jellemző földrajzi helyről.</span><span class="sxs-lookup"><span data-stu-id="2cc43-108">A sign-in from a geographic location that is not typical for the specific user, similar users, or the tenant.</span></span>

### <a name="azure-ad-identity-protection"></a><span data-ttu-id="2cc43-109">Azure AD Identity Protection</span><span class="sxs-lookup"><span data-stu-id="2cc43-109">Azure AD Identity Protection</span></span>
<span data-ttu-id="2cc43-110">A biztonsági modul az Azure Active Directoryban, amely a kockázati eseményekről és egy szervezet identitásait érintő lehetséges biztonsági rések egyesített nézetét biztosítja.</span><span class="sxs-lookup"><span data-stu-id="2cc43-110">A security module of Azure Active Directory that provides a consolidated view into risk events and potential vulnerabilities affecting an organization’s identities.</span></span>

### <a name="conditional-access"></a><span data-ttu-id="2cc43-111">Feltételes hozzáférés</span><span class="sxs-lookup"><span data-stu-id="2cc43-111">Conditional access</span></span>
<span data-ttu-id="2cc43-112">Egy házirend erőforrásokhoz való hozzáférés biztosítása érdekében.</span><span class="sxs-lookup"><span data-stu-id="2cc43-112">A policy for securing access to resources.</span></span> <span data-ttu-id="2cc43-113">Feltételes hozzáférési szabályai tárolódnak az Azure Active Directory és az erőforráshoz való hozzáférés megadása előtt az Azure AD értékeli ki.</span><span class="sxs-lookup"><span data-stu-id="2cc43-113">Conditional access rules are stored in the Azure Active Directory and are evaluated by Azure AD before granting access to the resource.</span></span>  <span data-ttu-id="2cc43-114">Például a szabályok a következők történő hozzáférés a felhasználó helye alapján eszköz állapotát, vagy a felhasználó a hitelesítési módszert.</span><span class="sxs-lookup"><span data-stu-id="2cc43-114">Example rules include restricting access based on user location, device health or user authentication method.</span></span>

### <a name="credentials"></a><span data-ttu-id="2cc43-115">Hitelesítő adatok</span><span class="sxs-lookup"><span data-stu-id="2cc43-115">Credentials</span></span>
<span data-ttu-id="2cc43-116">Azonosításra és igazolása helyi való hozzáférés és a hálózati erőforrásokhoz való használt azonosító adatokat.</span><span class="sxs-lookup"><span data-stu-id="2cc43-116">Information that includes identification and proof of identification that is used to gain access to local and network resources.</span></span> <span data-ttu-id="2cc43-117">A hitelesítő adatok többek között a felhasználónevek és jelszavak, az intelligens kártyák és a tanúsítványok.</span><span class="sxs-lookup"><span data-stu-id="2cc43-117">Examples of credentials are user names and passwords, smart cards, and certificates.</span></span>

### <a name="event"></a><span data-ttu-id="2cc43-118">Esemény</span><span class="sxs-lookup"><span data-stu-id="2cc43-118">Event</span></span>
<span data-ttu-id="2cc43-119">Az Azure Active Directoryban tevékenység egy olyan rekordot.</span><span class="sxs-lookup"><span data-stu-id="2cc43-119">A record of an activity in Azure Active Directory.</span></span>

### <a name="false-positive-risk-event"></a><span data-ttu-id="2cc43-120">A vakriasztások (kockázat esemény)</span><span class="sxs-lookup"><span data-stu-id="2cc43-120">False-positive (risk event)</span></span>
<span data-ttu-id="2cc43-121">A kockázat eseményállapot manuálisan Identity Protection felhasználó, amely azt jelzi, hogy a kockázati esemény történt a vizsgált, és a kockázati események megjelölt helytelenül lett beállítva.</span><span class="sxs-lookup"><span data-stu-id="2cc43-121">A risk event status set manually by an Identity Protection user, indicating that the risk event was investigated and was incorrectly flagged as a risk event.</span></span>

### <a name="identity"></a><span data-ttu-id="2cc43-122">Identitás</span><span class="sxs-lookup"><span data-stu-id="2cc43-122">Identity</span></span>
<span data-ttu-id="2cc43-123">Személy vagy vállalat hitelesítést, például a jelszóval vagy tanúsítvánnyal feltétel alapján történő ellenőrizni kell.</span><span class="sxs-lookup"><span data-stu-id="2cc43-123">A person or entity that must be verified by means of authentication, based on criteria such as password or a certificate.</span></span>

### <a name="identity-risk-event"></a><span data-ttu-id="2cc43-124">Identitás kockázat esemény</span><span class="sxs-lookup"><span data-stu-id="2cc43-124">Identity risk event</span></span>
<span data-ttu-id="2cc43-125">Az AAD esemény Identity Protection által rendellenes tevékenységként lett megjelölve, és arra utalhat, hogy identitás feltörték.</span><span class="sxs-lookup"><span data-stu-id="2cc43-125">AAD event that was flagged as anomalous by Identity Protection, and may indicate that an identity has been compromised.</span></span>

### <a name="ignored-risk-event"></a><span data-ttu-id="2cc43-126">Figyelmen kívül hagyva (kockázat esemény)</span><span class="sxs-lookup"><span data-stu-id="2cc43-126">Ignored (risk event)</span></span>
<span data-ttu-id="2cc43-127">A kockázat eseményállapot állítsa be manuálisan egy Identity Protection felhasználónak, amely azt jelzi, hogy a kockázati esemény nélkül a szervizelési művelet befejeződik.</span><span class="sxs-lookup"><span data-stu-id="2cc43-127">A risk event status set manually by an Identity Protection user, indicating that the risk event is closed without taking a remediation action.</span></span>

### <a name="impossible-travel-from-atypical-locations"></a><span data-ttu-id="2cc43-128">Lehetetlen odautazás bejelentkezés szokatlan helyekről</span><span class="sxs-lookup"><span data-stu-id="2cc43-128">Impossible travel from atypical locations</span></span>
<span data-ttu-id="2cc43-129">A kockázat ugyanaz a felhasználó két bejelentkezést észlelt, amennyiben legalább az egyik helyről a rendellenes bejelentkezési, és egyes esetekben a bejelentkezések közötti ideje rövidebb, mint a minimális időbe telne között fizikailag továbbítani által elindított esemény.</span><span class="sxs-lookup"><span data-stu-id="2cc43-129">A risk event triggered when two sign-ins for the same user are detected, where at least one of them is from an atypical sign-in location, and where the time between the sign-ins is shorter than the minimum time it would take to physically travel between these locations.</span></span>  

### <a name="investigation"></a><span data-ttu-id="2cc43-130">Vizsgálat</span><span class="sxs-lookup"><span data-stu-id="2cc43-130">Investigation</span></span>
<span data-ttu-id="2cc43-131">A tevékenységek, a naplókat, és olyan releváns információkat eldöntheti, hogy szervizelési vagy enyhítési lépések szükségesek, a kockázat eseményhez kapcsolódó véleményezési eljárás megértéséhez, ha, és hogyan identitásának biztonsági szempontból sérült, és a sérült biztonságú identitás használatának megismerése.</span><span class="sxs-lookup"><span data-stu-id="2cc43-131">The process of reviewing the activities, logs, and other relevant information related to a risk event to decide whether remediation or mitigation steps are necessary, understand if and how the identity was compromised, and understand how the compromised identity was used.</span></span>

### <a name="leaked-credentials"></a><span data-ttu-id="2cc43-132">Kiszivárgott hitelesítő adatok</span><span class="sxs-lookup"><span data-stu-id="2cc43-132">Leaked credentials</span></span>
<span data-ttu-id="2cc43-133">A kockázat aktuális felhasználói hitelesítő adatok (felhasználónév és jelszó) találhatók, a kutatói által nyilvánosan sötét webes közzétéve által elindított esemény.</span><span class="sxs-lookup"><span data-stu-id="2cc43-133">A risk event triggered when current user credentials (user name and password) are found posted publicly in the Dark   web by our researchers.</span></span>

### <a name="mitigation"></a><span data-ttu-id="2cc43-134">Kezelés</span><span class="sxs-lookup"><span data-stu-id="2cc43-134">Mitigation</span></span>
<span data-ttu-id="2cc43-135">Egy műveletet korlátozható vagy kiküszöbölheti a támadó lehetőségét a sérült biztonságú identitás vagy az eszköz visszaállítja a identitás vagy az eszköz biztonságos kihasználásához.</span><span class="sxs-lookup"><span data-stu-id="2cc43-135">An action to limit or eliminate the ability of an attacker to exploit a compromised identity or device without restoring the identity or device to a safe state.</span></span> <span data-ttu-id="2cc43-136">A megoldás nem oldja meg a identitás vagy az eszköz társított előző kockázati eseményekről.</span><span class="sxs-lookup"><span data-stu-id="2cc43-136">A mitigation does not resolve previous risk events associated with the identity or device.</span></span>

### <a name="multi-factor-authentication"></a><span data-ttu-id="2cc43-137">Multi-Factor Authentication</span><span class="sxs-lookup"><span data-stu-id="2cc43-137">Multi-factor authentication</span></span>
<span data-ttu-id="2cc43-138">A hitelesítési módszert, amelyhez két vagy több hitelesítési módszert, amelyek magukban foglalhatják valamit a felhasználó rendelkezik, ez a tanúsítvány; valamit a felhasználó ismer, például felhasználóneveket, jelszavakat vagy fázis kifejezések; fizikai attribútumait, például egy ujjlenyomat; és személyes attribútumait, például a személyes aláírás.</span><span class="sxs-lookup"><span data-stu-id="2cc43-138">An authentication method that requires two or more authentication methods, which may include something the user has, such a certificate; something the user knows, such as user names, passwords, or pass phrases; physical attributes, such as a thumbprint; and personal attributes, such as a personal signature.</span></span>

### <a name="offline-detection"></a><span data-ttu-id="2cc43-139">Kapcsolat nélküli észlelése</span><span class="sxs-lookup"><span data-stu-id="2cc43-139">Offline detection</span></span>
<span data-ttu-id="2cc43-140">A rendellenességek észlelését és az esemény például a bejelentkezési kísérlet kockázatát értékelése után a tényt, az esemény, amely már elvégzett észlelését.</span><span class="sxs-lookup"><span data-stu-id="2cc43-140">The detection of anomalies and evaluation of the risk of an event such as sign-in attempt after the fact, for an event that has already happened.</span></span>

### <a name="policy-condition"></a><span data-ttu-id="2cc43-141">Házirend feltétel</span><span class="sxs-lookup"><span data-stu-id="2cc43-141">Policy condition</span></span>
<span data-ttu-id="2cc43-142">Egy olyan biztonsági házirendet, amely meghatározza az entitások (csoportok, felhasználók, alkalmazások, eszközök, eszközök állapotai, IP-címtartományok, ügyféltípusokat) egy része szerepel a házirendben, illetve tiltani szeretné a azt.</span><span class="sxs-lookup"><span data-stu-id="2cc43-142">A part of a security policy which defines the entities (groups, users, apps, device platforms, Device states, IP ranges, client types) included in the policy or excluded from it.</span></span>

### <a name="policy-rule"></a><span data-ttu-id="2cc43-143">Házirend szabályai</span><span class="sxs-lookup"><span data-stu-id="2cc43-143">Policy rule</span></span>
<span data-ttu-id="2cc43-144">Egy olyan biztonsági házirendet, amely leírja a körülmények között, indítsa el a szabályzatot, és a házirend kiváltásakor végrehajtott műveleteket részét.</span><span class="sxs-lookup"><span data-stu-id="2cc43-144">The part of a security policy which describes the circumstances that would trigger the policy, and the actions taken when the policy is triggered.</span></span>

### <a name="prevention"></a><span data-ttu-id="2cc43-145">Megelőzés</span><span class="sxs-lookup"><span data-stu-id="2cc43-145">Prevention</span></span>
<span data-ttu-id="2cc43-146">Ahhoz, hogy a sérülés visszaélés identitás vagy eszköz a szervezet a művelet gyanús, vagy tudja, hogy utaló jeleket.</span><span class="sxs-lookup"><span data-stu-id="2cc43-146">An action to prevent damage to the organization through abuse of an identity or device suspected or know to be compromised.</span></span> <span data-ttu-id="2cc43-147">Egy megelőzési művelet nem biztonságos, az eszköz vagy az identitás, és nem oldja meg az előző kockázati eseményekről.</span><span class="sxs-lookup"><span data-stu-id="2cc43-147">A prevention action does not secure the device or identity, and does not resolve previous risk events.</span></span>

### <a name="privileged-user"></a><span data-ttu-id="2cc43-148">Kiemelt (felhasználó)</span><span class="sxs-lookup"><span data-stu-id="2cc43-148">Privileged (user)</span></span>
<span data-ttu-id="2cc43-149">Egy olyan felhasználó, a kockázat esemény időpontjában volt egy vagy több erőforrás állandó vagy ideiglenes rendszergazdai engedélyekkel az Azure Active Directoryban, például egy globális rendszergazda számlázási rendszergazda, a szolgáltatás-rendszergazdát, a felhasználó rendszergazda és a jelszókezelő.</span><span class="sxs-lookup"><span data-stu-id="2cc43-149">A user that at the time of a risk event, had permanent or temporary admin permissions to one or more resource in Azure Active Directory, such as a Global Administrator, Billing Administrator, Service Administrator, User administrator, and Password Administrator.</span></span> 

### <a name="real-time"></a><span data-ttu-id="2cc43-150">Valós idejű</span><span class="sxs-lookup"><span data-stu-id="2cc43-150">Real-time</span></span>
<span data-ttu-id="2cc43-151">Tekintse meg a valós idejű észlelése.</span><span class="sxs-lookup"><span data-stu-id="2cc43-151">See Real-time detection.</span></span>

### <a name="real-time-detection"></a><span data-ttu-id="2cc43-152">Valós idejű észlelése</span><span class="sxs-lookup"><span data-stu-id="2cc43-152">Real-time detection</span></span>
<span data-ttu-id="2cc43-153">A tegye a rendellenességek észlelését és az esemény például a bejelentkezési kísérlet az esemény előtt kockázatbecslés engedélyezett a folytatáshoz.</span><span class="sxs-lookup"><span data-stu-id="2cc43-153">The detection of anomalies and evaluation of the risk of an event such as sign-in attempt before the event is allowed to proceed.</span></span>

### <a name="remediated-risk-event"></a><span data-ttu-id="2cc43-154">Kijavítva (kockázat esemény)</span><span class="sxs-lookup"><span data-stu-id="2cc43-154">Remediated (risk event)</span></span>
<span data-ttu-id="2cc43-155">Azonosító adatok védelmét, a kockázat esemény a szabványos szervizelési művelet használata az ilyen típusú kockázat esemény lett kijavítva jelző által automatikusan beállított kockázati esemény állapota.</span><span class="sxs-lookup"><span data-stu-id="2cc43-155">A risk event status set automatically by Identity Protection, indicating that the risk event was remediated using the standard remediation action for this type of risk event.</span></span> <span data-ttu-id="2cc43-156">Például amikor a felhasználó jelszavát, sok kockázati események, amelyek jelzik, hogy esetleges biztonsági sérülés esetén a korábbi jelszót vannak pótolja őket automatikusan.</span><span class="sxs-lookup"><span data-stu-id="2cc43-156">For example, when the user password is reset, many risk events that indicate that the previous password was compromised are automatically remediated.</span></span>

### <a name="remediation"></a><span data-ttu-id="2cc43-157">Szervizkiszolgáló</span><span class="sxs-lookup"><span data-stu-id="2cc43-157">Remediation</span></span>
<span data-ttu-id="2cc43-158">Egy műveletet, biztonságos identitás vagy egy eszköz, amely korábban gyanús vagy megsértik ismert.</span><span class="sxs-lookup"><span data-stu-id="2cc43-158">An action to secure an identity or a device that were previously suspected or known to be compromised.</span></span> <span data-ttu-id="2cc43-159">A javítási művelet visszaállítja a identitás vagy az eszköz biztonságos, és oldja fel a identitás vagy az eszköz társított előző kockázati eseményekről.</span><span class="sxs-lookup"><span data-stu-id="2cc43-159">A remediation action restores the identity or device to a safe state, and resolves previous risk events associated with the identity or device.</span></span>

### <a name="resolved-risk-event"></a><span data-ttu-id="2cc43-160">Megoldott (kockázat esemény)</span><span class="sxs-lookup"><span data-stu-id="2cc43-160">Resolved (risk event)</span></span>
<span data-ttu-id="2cc43-161">A kockázat eseményállapot Identity Protection felhasználó manuálisan állítsa be, jelezve, hogy a felhasználó elvégez egy megfelelő szervizelési művelet kívül Identity Protection, és a kockázati esemény tekintendő zárva.</span><span class="sxs-lookup"><span data-stu-id="2cc43-161">A risk event status set manually by an Identity Protection user, indicating that the user took an appropriate remediation action outside Identity Protection, and that the risk event should be considered closed.</span></span>

### <a name="risk-event-status"></a><span data-ttu-id="2cc43-162">Kockázati eseményállapot</span><span class="sxs-lookup"><span data-stu-id="2cc43-162">Risk event status</span></span>
<span data-ttu-id="2cc43-163">A kockázati események tulajdonságának, amely jelzi, hogy aktív-e az esemény, és ha bezárul, akkor lezárásának okát.</span><span class="sxs-lookup"><span data-stu-id="2cc43-163">A property of a risk event, indicating whether the event is active, and if closed, the reason for closing it.</span></span>

### <a name="risk-event-type"></a><span data-ttu-id="2cc43-164">Kockázati esemény típusa</span><span class="sxs-lookup"><span data-stu-id="2cc43-164">Risk event type</span></span>
<span data-ttu-id="2cc43-165">A kockázat esemény, az esemény akkor, ha kockázatos okozó anomáliadetektálási típusának kategóriáját.</span><span class="sxs-lookup"><span data-stu-id="2cc43-165">A category for the risk event, indicating the type of anomaly that caused the event to be considered risky.</span></span>

### <a name="risk-level-risk-event"></a><span data-ttu-id="2cc43-166">Kockázati szint (kockázat esemény)</span><span class="sxs-lookup"><span data-stu-id="2cc43-166">Risk level (risk event)</span></span>
<span data-ttu-id="2cc43-167">Identity Protection felhasználók priorizálhatja azokat a műveleteket a kockázat esemény súlyossága megjelölése (magas, közepes vagy alacsony) tartanak a kockázatok csökkentése a szervezet számára.</span><span class="sxs-lookup"><span data-stu-id="2cc43-167">An indication (High, Medium, or Low) of the severity of the risk event to help Identity Protection users prioritize the actions they take to reduce the risk to their organization.</span></span> 

### <a name="risk-level-sign-in"></a><span data-ttu-id="2cc43-168">Kockázati szint (bejelentkezés)</span><span class="sxs-lookup"><span data-stu-id="2cc43-168">Risk level (sign-in)</span></span>
<span data-ttu-id="2cc43-169">Jelzése (magas, közepes vagy alacsony) annak a valószínűségét, hogy az egy adott bejelentkezés, valaki más próbál használni, a felhasználó identitását.</span><span class="sxs-lookup"><span data-stu-id="2cc43-169">An indication (High, Medium, or Low) of the likelihood that for a specific sign-in, someone else is attempting to use the user’s identity.</span></span>

### <a name="risk-level-user-compromise"></a><span data-ttu-id="2cc43-170">Kockázati szint (felhasználói sérült biztonság esetén)</span><span class="sxs-lookup"><span data-stu-id="2cc43-170">Risk level (user compromise)</span></span>
<span data-ttu-id="2cc43-171">Jelzése (magas, közepes vagy alacsony) annak a valószínűségét, hogy identitás megtámadták.</span><span class="sxs-lookup"><span data-stu-id="2cc43-171">An indication (High, Medium, or Low) of the likelihood that an identity has been compromised.</span></span>

### <a name="risk-level-vulnerability"></a><span data-ttu-id="2cc43-172">Kockázati szint (biztonsági rés)</span><span class="sxs-lookup"><span data-stu-id="2cc43-172">Risk level (vulnerability)</span></span>
<span data-ttu-id="2cc43-173">A biztonsági rés priorizálhatja azokat a műveleteket Identity Protection felhasználók súlyossága megjelölése (magas, közepes vagy alacsony) tartanak a kockázatok csökkentése a szervezet számára.</span><span class="sxs-lookup"><span data-stu-id="2cc43-173">An indication (High, Medium, or Low) of the severity of the vulnerability to help Identity Protection users prioritize the actions they take to reduce the risk to their organization.</span></span>

### <a name="secure-identity"></a><span data-ttu-id="2cc43-174">Biztonságos (identitás)</span><span class="sxs-lookup"><span data-stu-id="2cc43-174">Secure (identity)</span></span>
<span data-ttu-id="2cc43-175">Például a jelszó módosítása vagy a gép, egy biztonsági szempontból sértetlen állapotának visszaállításához a potenciálisan veszélyeztetett identitás különösen szervizelési művelet végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="2cc43-175">Take remediation action such as a password change or machine reimaging to restore a potentially compromised identity to an uncompromised state.</span></span>

### <a name="security-policy"></a><span data-ttu-id="2cc43-176">Biztonsági házirend</span><span class="sxs-lookup"><span data-stu-id="2cc43-176">Security policy</span></span>
<span data-ttu-id="2cc43-177">Szabályok és az állapot gyűjteménye.</span><span class="sxs-lookup"><span data-stu-id="2cc43-177">A collection of policy rules and condition.</span></span> <span data-ttu-id="2cc43-178">Egy házirend entitások, például a felhasználók, csoportok, alkalmazások, eszközök, eszközök, eszköz állapotok, IP-címtartományok és Auth2.0 ügyféltípusokat alkalmazhatja.</span><span class="sxs-lookup"><span data-stu-id="2cc43-178">A policy can be applied to entities such as users, groups, apps, devices, device platforms, device states, IP ranges, and Auth2.0 client types.</span></span> <span data-ttu-id="2cc43-179">A házirend engedélyezve van, amikor a házirendben entitás erőforrás jogkivonatot ad ki lesznek kiértékelve.</span><span class="sxs-lookup"><span data-stu-id="2cc43-179">When a policy is enabled, it is evaluated whenever an entity included in the policy is issued a token for a resource.</span></span>

### <a name="sign-in-v"></a><span data-ttu-id="2cc43-180">Jelentkezzen be (v)</span><span class="sxs-lookup"><span data-stu-id="2cc43-180">Sign in (v)</span></span>
<span data-ttu-id="2cc43-181">A hitelesítés az identitás, az Azure Active Directoryban.</span><span class="sxs-lookup"><span data-stu-id="2cc43-181">To authenticate to an identity in Azure Active Directory.</span></span>

### <a name="sign-in-n"></a><span data-ttu-id="2cc43-182">Bejelentkezés (n)</span><span class="sxs-lookup"><span data-stu-id="2cc43-182">Sign-in (n)</span></span>
<span data-ttu-id="2cc43-183">A folyamat vagy egy Azure Active Directory és az esemény, amely a művelet rögzíti a személyazonossága hitelesítésének művelettel.</span><span class="sxs-lookup"><span data-stu-id="2cc43-183">The process or action of authenticating an identity in Azure Active Directory, and the event that captures this operation.</span></span>

### <a name="sign-in-from-anonymous-ip-address"></a><span data-ttu-id="2cc43-184">Bejelentkezés a névtelen IP-cím</span><span class="sxs-lookup"><span data-stu-id="2cc43-184">Sign-in from anonymous IP address</span></span>
<span data-ttu-id="2cc43-185">A kockázat esemény után egy sikeres bejelentkezés névtelen proxy IP-címként azonosított IP-címről következik be.</span><span class="sxs-lookup"><span data-stu-id="2cc43-185">A risk event triggered after a successful sign-in from IP address that has been identified as an anonymous proxy IP address.</span></span>

### <a name="sign-in-from-infected-device"></a><span data-ttu-id="2cc43-186">Bejelentkezés a fertőzött eszköz</span><span class="sxs-lookup"><span data-stu-id="2cc43-186">Sign-in from infected device</span></span>
<span data-ttu-id="2cc43-187">A kockázat a bejelentkezés egy IP-címet, amelyről ismert, hogy az egy vagy több feltört eszközök, amelyek aktívan próbál kommunikálni egy botnetes kiszolgálóhoz használható származik által elindított esemény.</span><span class="sxs-lookup"><span data-stu-id="2cc43-187">A risk event triggered when a sign-in originates from an IP address which is known to be used by one or more compromised devices, which are actively attempting to communicate with a bot server.</span></span>

### <a name="sign-in-from-ip-address-with-suspicious-activity"></a><span data-ttu-id="2cc43-188">Jelentkezzen be a következő IP-gyanús tevékenység</span><span class="sxs-lookup"><span data-stu-id="2cc43-188">Sign-in from IP address with suspicious activity</span></span>
<span data-ttu-id="2cc43-189">A kockázat az esemény akkor váltódik ki, miután egy sikeres bejelentkezés IP cím nagyszámú sikertelen bejelentkezési kísérlet között több felhasználói fiókot egy rövid időtartamra vonatkozóan.</span><span class="sxs-lookup"><span data-stu-id="2cc43-189">A risk event triggered after a successful sign-in from an IP address with a high number of failed login attempts across multiple user accounts over a short period of time.</span></span>

### <a name="sign-in-from-unfamiliar-location"></a><span data-ttu-id="2cc43-190">Bejelentkezés ismeretlen helyről</span><span class="sxs-lookup"><span data-stu-id="2cc43-190">Sign-in from unfamiliar location</span></span>
<span data-ttu-id="2cc43-191">A kockázat felhasználó sikeresen jelentkezik be egy új helyről (IP, szélesség/hosszúsági és ASN) által elindított esemény.</span><span class="sxs-lookup"><span data-stu-id="2cc43-191">A risk event triggered when a user successfully signs in from a new location (IP, Latitude/Longitude and ASN).</span></span>

### <a name="sign-in-risk"></a><span data-ttu-id="2cc43-192">Bejelentkezési kockázata</span><span class="sxs-lookup"><span data-stu-id="2cc43-192">Sign-in risk</span></span>
<span data-ttu-id="2cc43-193">Tekintse meg a kockázati szintjét (bejelentkezés)</span><span class="sxs-lookup"><span data-stu-id="2cc43-193">See Risk level (sign-in)</span></span>

### <a name="sign-in-risk-policy"></a><span data-ttu-id="2cc43-194">Bejelentkezési kockázat házirendnek</span><span class="sxs-lookup"><span data-stu-id="2cc43-194">Sign-in risk policy</span></span>
<span data-ttu-id="2cc43-195">Feltételes hozzáférési szabályzatot, amely egy adott jelentkezik be a kockázat és azok mérséklési előre meghatározott feltételt és a szabályok alapján alkalmazza.</span><span class="sxs-lookup"><span data-stu-id="2cc43-195">A conditional access policy that evaluates the risk to a specific sign-in and applies mitigations based on predefined conditions and rules.</span></span>

### <a name="user-compromise-risk"></a><span data-ttu-id="2cc43-196">Felhasználó biztonsági sérülés kockázata</span><span class="sxs-lookup"><span data-stu-id="2cc43-196">User compromise risk</span></span>
<span data-ttu-id="2cc43-197">Tekintse meg a kockázati szintjét (felhasználói sérült biztonság esetén)</span><span class="sxs-lookup"><span data-stu-id="2cc43-197">See Risk level (user compromise)</span></span>

### <a name="user-risk"></a><span data-ttu-id="2cc43-198">Felhasználói kockázata</span><span class="sxs-lookup"><span data-stu-id="2cc43-198">User risk</span></span>
<span data-ttu-id="2cc43-199">Tekintse meg a kockázati szintjét (felhasználói sérült biztonság esetén).</span><span class="sxs-lookup"><span data-stu-id="2cc43-199">See Risk level (user compromise).</span></span>

### <a name="user-risk-policy"></a><span data-ttu-id="2cc43-200">Felhasználói kockázat házirendnek</span><span class="sxs-lookup"><span data-stu-id="2cc43-200">User risk policy</span></span>
<span data-ttu-id="2cc43-201">Feltételes hozzáférési szabályzatot, amely úgy ítéli meg, a bejelentkezés, és azok mérséklési előre meghatározott feltételt és a szabályok alapján alkalmazza.</span><span class="sxs-lookup"><span data-stu-id="2cc43-201">A conditional access policy that considers the sign-in and applies mitigations based on predefined conditions and rules.</span></span>

### <a name="users-flagged-for-risk"></a><span data-ttu-id="2cc43-202">Kockázatosként megjelölt felhasználók</span><span class="sxs-lookup"><span data-stu-id="2cc43-202">Users flagged for risk</span></span>
<span data-ttu-id="2cc43-203">Kockázati eseményekről, amelyek aktív vagy szervizelt rendelkező felhasználók</span><span class="sxs-lookup"><span data-stu-id="2cc43-203">Users that have risk events which are either active or remediated</span></span>

### <a name="vulnerability"></a><span data-ttu-id="2cc43-204">A biztonsági rés</span><span class="sxs-lookup"><span data-stu-id="2cc43-204">Vulnerability</span></span>
<span data-ttu-id="2cc43-205">Egy konfigurációs vagy az Azure Active Directoryban, így ki vannak téve a biztonsági rések a könyvtár feltétel vagy fenyegetéseket.</span><span class="sxs-lookup"><span data-stu-id="2cc43-205">A configuration or condition in Azure Active Directory which makes the directory susceptible to exploits or threats.</span></span>

## <a name="see-also"></a><span data-ttu-id="2cc43-206">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="2cc43-206">See also</span></span>
* [<span data-ttu-id="2cc43-207">Az Azure Active Directory azonosító adatok védelmét</span><span class="sxs-lookup"><span data-stu-id="2cc43-207">Azure Active Directory Identity Protection</span></span>](active-directory-identityprotection.md)

