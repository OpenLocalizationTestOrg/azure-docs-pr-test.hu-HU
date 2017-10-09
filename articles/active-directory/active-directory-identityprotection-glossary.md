---
title: "Active Directory Identity Protection-szószedet aaaAzure |} Microsoft Docs"
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
ms.openlocfilehash: ff2e96d20e2a3f1df24b78e66be5a0c6807e60a3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-identity-protection-glossary"></a><span data-ttu-id="b8078-104">Az Azure Active Directory-Identity Protection-szószedet</span><span class="sxs-lookup"><span data-stu-id="b8078-104">Azure Active Directory Identity Protection Glossary</span></span>
### <a name="at-risk-user"></a><span data-ttu-id="b8078-105">Fennáll a veszélye (felhasználó)</span><span class="sxs-lookup"><span data-stu-id="b8078-105">At risk (User)</span></span>
<span data-ttu-id="b8078-106">A felhasználó egy vagy több aktív kockázati események.</span><span class="sxs-lookup"><span data-stu-id="b8078-106">A user with one or more active risk events.</span></span> 

### <a name="atypical-sign-in-location"></a><span data-ttu-id="b8078-107">Bejelentkezés szokatlan bejelentkezési helye</span><span class="sxs-lookup"><span data-stu-id="b8078-107">Atypical sign-in location</span></span>
<span data-ttu-id="b8078-108">A bejelentkezés, amely nincs a hello konkrét felhasználó, hasonló felhasználók vagy hello bérlői jellemző földrajzi helyről.</span><span class="sxs-lookup"><span data-stu-id="b8078-108">A sign-in from a geographic location that is not typical for hello specific user, similar users, or hello tenant.</span></span>

### <a name="azure-ad-identity-protection"></a><span data-ttu-id="b8078-109">Azure AD Identity Protection</span><span class="sxs-lookup"><span data-stu-id="b8078-109">Azure AD Identity Protection</span></span>
<span data-ttu-id="b8078-110">A biztonsági modul az Azure Active Directoryban, amely a kockázati eseményekről és egy szervezet identitásait érintő lehetséges biztonsági rések egyesített nézetét biztosítja.</span><span class="sxs-lookup"><span data-stu-id="b8078-110">A security module of Azure Active Directory that provides a consolidated view into risk events and potential vulnerabilities affecting an organization’s identities.</span></span>

### <a name="conditional-access"></a><span data-ttu-id="b8078-111">Feltételes hozzáférés</span><span class="sxs-lookup"><span data-stu-id="b8078-111">Conditional access</span></span>
<span data-ttu-id="b8078-112">A biztonságos hozzáférés tooresources egy házirendet.</span><span class="sxs-lookup"><span data-stu-id="b8078-112">A policy for securing access tooresources.</span></span> <span data-ttu-id="b8078-113">Feltételes hozzáférési szabályai hello Azure Active Directoryban tárolja, és az Azure AD hozzáférési toohello erőforrás megelőzően értékeli ki.</span><span class="sxs-lookup"><span data-stu-id="b8078-113">Conditional access rules are stored in hello Azure Active Directory and are evaluated by Azure AD before granting access toohello resource.</span></span>  <span data-ttu-id="b8078-114">Például a szabályok a következők történő hozzáférés a felhasználó helye alapján eszköz állapotát, vagy a felhasználó a hitelesítési módszert.</span><span class="sxs-lookup"><span data-stu-id="b8078-114">Example rules include restricting access based on user location, device health or user authentication method.</span></span>

### <a name="credentials"></a><span data-ttu-id="b8078-115">Hitelesítő adatok</span><span class="sxs-lookup"><span data-stu-id="b8078-115">Credentials</span></span>
<span data-ttu-id="b8078-116">Azonosító és az azonosítót a használt toogain toolocal és a hálózati erőforrások eléréséhez igazolása tartalmaz információt.</span><span class="sxs-lookup"><span data-stu-id="b8078-116">Information that includes identification and proof of identification that is used toogain access toolocal and network resources.</span></span> <span data-ttu-id="b8078-117">A hitelesítő adatok többek között a felhasználónevek és jelszavak, az intelligens kártyák és a tanúsítványok.</span><span class="sxs-lookup"><span data-stu-id="b8078-117">Examples of credentials are user names and passwords, smart cards, and certificates.</span></span>

### <a name="event"></a><span data-ttu-id="b8078-118">Esemény</span><span class="sxs-lookup"><span data-stu-id="b8078-118">Event</span></span>
<span data-ttu-id="b8078-119">Az Azure Active Directoryban tevékenység egy olyan rekordot.</span><span class="sxs-lookup"><span data-stu-id="b8078-119">A record of an activity in Azure Active Directory.</span></span>

### <a name="false-positive-risk-event"></a><span data-ttu-id="b8078-120">A vakriasztások (kockázat esemény)</span><span class="sxs-lookup"><span data-stu-id="b8078-120">False-positive (risk event)</span></span>
<span data-ttu-id="b8078-121">A kockázat eseményállapot manuálisan Identity Protection felhasználó, jelezve, hogy hello kockázat esemény vizsgált lett, és a kockázati események megjelölt helytelenül lett beállítva.</span><span class="sxs-lookup"><span data-stu-id="b8078-121">A risk event status set manually by an Identity Protection user, indicating that hello risk event was investigated and was incorrectly flagged as a risk event.</span></span>

### <a name="identity"></a><span data-ttu-id="b8078-122">Identitás</span><span class="sxs-lookup"><span data-stu-id="b8078-122">Identity</span></span>
<span data-ttu-id="b8078-123">Személy vagy vállalat hitelesítést, például a jelszóval vagy tanúsítvánnyal feltétel alapján történő ellenőrizni kell.</span><span class="sxs-lookup"><span data-stu-id="b8078-123">A person or entity that must be verified by means of authentication, based on criteria such as password or a certificate.</span></span>

### <a name="identity-risk-event"></a><span data-ttu-id="b8078-124">Identitás kockázat esemény</span><span class="sxs-lookup"><span data-stu-id="b8078-124">Identity risk event</span></span>
<span data-ttu-id="b8078-125">Az AAD esemény Identity Protection által rendellenes tevékenységként lett megjelölve, és arra utalhat, hogy identitás feltörték.</span><span class="sxs-lookup"><span data-stu-id="b8078-125">AAD event that was flagged as anomalous by Identity Protection, and may indicate that an identity has been compromised.</span></span>

### <a name="ignored-risk-event"></a><span data-ttu-id="b8078-126">Figyelmen kívül hagyva (kockázat esemény)</span><span class="sxs-lookup"><span data-stu-id="b8078-126">Ignored (risk event)</span></span>
<span data-ttu-id="b8078-127">A kockázat eseményállapot Identity Protection felhasználó, jelezve, hogy hello kockázat esemény le van zárva, a szervizelési műveletek nélkül manuálisan állítsa be.</span><span class="sxs-lookup"><span data-stu-id="b8078-127">A risk event status set manually by an Identity Protection user, indicating that hello risk event is closed without taking a remediation action.</span></span>

### <a name="impossible-travel-from-atypical-locations"></a><span data-ttu-id="b8078-128">Lehetetlen odautazás bejelentkezés szokatlan helyekről</span><span class="sxs-lookup"><span data-stu-id="b8078-128">Impossible travel from atypical locations</span></span>
<span data-ttu-id="b8078-129">A kockázat hello ugyanahhoz a felhasználóhoz észlelt, amikor legalább az egyik van helyről a rendellenes bejelentkezési, és ha hello közötti hello bejelentkezések ideje rövidebb, mint a minimális hello idő két bejelentkezést időt vesz igénybe toophysically által elindított esemény haladnak, ezek között helyek.</span><span class="sxs-lookup"><span data-stu-id="b8078-129">A risk event triggered when two sign-ins for hello same user are detected, where at least one of them is from an atypical sign-in location, and where hello time between hello sign-ins is shorter than hello minimum time it would take toophysically travel between these locations.</span></span>  

### <a name="investigation"></a><span data-ttu-id="b8078-130">Vizsgálat</span><span class="sxs-lookup"><span data-stu-id="b8078-130">Investigation</span></span>
<span data-ttu-id="b8078-131">hello hello tevékenységek, a naplókat, és olyan releváns információkat véleményezési eljárás kapcsolódó tooa kockázat esemény toodecide szervizelési vagy enyhítési lépések szükségesek, hogy, megértéséhez Ha, és hogyan hello identitás biztonsági szempontból sérült, és megismerje hogyan hello sérült biztonságú azonosító lett megadva.</span><span class="sxs-lookup"><span data-stu-id="b8078-131">hello process of reviewing hello activities, logs, and other relevant information related tooa risk event toodecide whether remediation or mitigation steps are necessary, understand if and how hello identity was compromised, and understand how hello compromised identity was used.</span></span>

### <a name="leaked-credentials"></a><span data-ttu-id="b8078-132">Kiszivárgott hitelesítő adatok</span><span class="sxs-lookup"><span data-stu-id="b8078-132">Leaked credentials</span></span>
<span data-ttu-id="b8078-133">A kockázat aktuális felhasználói hitelesítő adatok (felhasználónév és jelszó) találhatók által elindított esemény nyilvánosan hello sötét webes a kutatói által közzétett.</span><span class="sxs-lookup"><span data-stu-id="b8078-133">A risk event triggered when current user credentials (user name and password) are found posted publicly in hello Dark   web by our researchers.</span></span>

### <a name="mitigation"></a><span data-ttu-id="b8078-134">Kezelés</span><span class="sxs-lookup"><span data-stu-id="b8078-134">Mitigation</span></span>
<span data-ttu-id="b8078-135">Egy művelet toolimit vagy hello azon képessége, egy támadó tooexploit egy sérült biztonságú identitás vagy az eszköz kiküszöbölése hello identitás vagy az eszköz tooa biztonságos állapot visszaállítása nélkül.</span><span class="sxs-lookup"><span data-stu-id="b8078-135">An action toolimit or eliminate hello ability of an attacker tooexploit a compromised identity or device without restoring hello identity or device tooa safe state.</span></span> <span data-ttu-id="b8078-136">A megoldás nem oldja meg a társított hello identitás vagy az eszköz korábbi kockázati eseményekről.</span><span class="sxs-lookup"><span data-stu-id="b8078-136">A mitigation does not resolve previous risk events associated with hello identity or device.</span></span>

### <a name="multi-factor-authentication"></a><span data-ttu-id="b8078-137">Multi-Factor Authentication</span><span class="sxs-lookup"><span data-stu-id="b8078-137">Multi-factor authentication</span></span>
<span data-ttu-id="b8078-138">A hitelesítési módszert, amelyhez két vagy több hitelesítési módszert, amelyek magukban foglalhatják valami hello felhasználó rendelkezik-e, ez a tanúsítvány; valami hello felhasználó ismer, például felhasználóneveket, jelszavakat vagy fázis kifejezések; fizikai attribútumait, például egy ujjlenyomat; és személyes attribútumait, például a személyes aláírás.</span><span class="sxs-lookup"><span data-stu-id="b8078-138">An authentication method that requires two or more authentication methods, which may include something hello user has, such a certificate; something hello user knows, such as user names, passwords, or pass phrases; physical attributes, such as a thumbprint; and personal attributes, such as a personal signature.</span></span>

### <a name="offline-detection"></a><span data-ttu-id="b8078-139">Kapcsolat nélküli észlelése</span><span class="sxs-lookup"><span data-stu-id="b8078-139">Offline detection</span></span>
<span data-ttu-id="b8078-140">hello észlelési rendellenességek észlelését és az értékelés hello kockázatát az esemény például a bejelentkezési kísérlet után hello ténynek megfelelő esemény, amely már elvégzett.</span><span class="sxs-lookup"><span data-stu-id="b8078-140">hello detection of anomalies and evaluation of hello risk of an event such as sign-in attempt after hello fact, for an event that has already happened.</span></span>

### <a name="policy-condition"></a><span data-ttu-id="b8078-141">Házirend feltétel</span><span class="sxs-lookup"><span data-stu-id="b8078-141">Policy condition</span></span>
<span data-ttu-id="b8078-142">A biztonsági házirend hello entitások (csoportok, felhasználók, alkalmazások, eszközök, eszközök állapotai, IP-címtartományok, ügyféltípusokat) hello házirendben, illetve tiltani szeretné a azt meghatározó része.</span><span class="sxs-lookup"><span data-stu-id="b8078-142">A part of a security policy which defines hello entities (groups, users, apps, device platforms, Device states, IP ranges, client types) included in hello policy or excluded from it.</span></span>

### <a name="policy-rule"></a><span data-ttu-id="b8078-143">Házirend szabályai</span><span class="sxs-lookup"><span data-stu-id="b8078-143">Policy rule</span></span>
<span data-ttu-id="b8078-144">egy biztonsági házirendet, amely ismerteti, indítsa el hello házirend, és hello házirend kiváltásakor hello műveleteit hello körülmények hello részét.</span><span class="sxs-lookup"><span data-stu-id="b8078-144">hello part of a security policy which describes hello circumstances that would trigger hello policy, and hello actions taken when hello policy is triggered.</span></span>

### <a name="prevention"></a><span data-ttu-id="b8078-145">Megelőzés</span><span class="sxs-lookup"><span data-stu-id="b8078-145">Prevention</span></span>
<span data-ttu-id="b8078-146">A művelet tooprevent kárt toohello munkahely visszaélés keresztül identitás vagy eszköz gyanús vagy sérült toobe tudja.</span><span class="sxs-lookup"><span data-stu-id="b8078-146">An action tooprevent damage toohello organization through abuse of an identity or device suspected or know toobe compromised.</span></span> <span data-ttu-id="b8078-147">Egy megelőzési művelet hello eszköz vagy identitás nem biztonságos, és nem oldja meg az előző kockázati eseményekről.</span><span class="sxs-lookup"><span data-stu-id="b8078-147">A prevention action does not secure hello device or identity, and does not resolve previous risk events.</span></span>

### <a name="privileged-user"></a><span data-ttu-id="b8078-148">Kiemelt (felhasználó)</span><span class="sxs-lookup"><span data-stu-id="b8078-148">Privileged (user)</span></span>
<span data-ttu-id="b8078-149">Egy olyan felhasználó, a kockázat esemény hello idején volt állandó vagy ideiglenes rendszergazdai engedélyek tooone vagy további erőforrás az Azure Active Directoryban, például egy globális rendszergazda számlázási rendszergazda, a szolgáltatás-rendszergazdát, a felhasználó rendszergazda és a jelszó Rendszergazda.</span><span class="sxs-lookup"><span data-stu-id="b8078-149">A user that at hello time of a risk event, had permanent or temporary admin permissions tooone or more resource in Azure Active Directory, such as a Global Administrator, Billing Administrator, Service Administrator, User administrator, and Password Administrator.</span></span> 

### <a name="real-time"></a><span data-ttu-id="b8078-150">Valós idejű</span><span class="sxs-lookup"><span data-stu-id="b8078-150">Real-time</span></span>
<span data-ttu-id="b8078-151">Tekintse meg a valós idejű észlelése.</span><span class="sxs-lookup"><span data-stu-id="b8078-151">See Real-time detection.</span></span>

### <a name="real-time-detection"></a><span data-ttu-id="b8078-152">Valós idejű észlelése</span><span class="sxs-lookup"><span data-stu-id="b8078-152">Real-time detection</span></span>
<span data-ttu-id="b8078-153">hello tegye a rendellenességek észlelését és az esemény például a bejelentkezési kísérlet hello esemény előtt hello kockázatbecslés tooproceed engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="b8078-153">hello detection of anomalies and evaluation of hello risk of an event such as sign-in attempt before hello event is allowed tooproceed.</span></span>

### <a name="remediated-risk-event"></a><span data-ttu-id="b8078-154">Kijavítva (kockázat esemény)</span><span class="sxs-lookup"><span data-stu-id="b8078-154">Remediated (risk event)</span></span>
<span data-ttu-id="b8078-155">Azonosító adatok védelmét, jelezve, hogy hello kockázat esemény lett kijavítva hello szabványos szervizelési művelet használata az ilyen típusú kockázat esemény által automatikusan beállított kockázati esemény állapota.</span><span class="sxs-lookup"><span data-stu-id="b8078-155">A risk event status set automatically by Identity Protection, indicating that hello risk event was remediated using hello standard remediation action for this type of risk event.</span></span> <span data-ttu-id="b8078-156">Például az hello felhasználó jelszavát, amikor sok kockázati események, amelyek jelzik, hogy hello korábbi jelszó biztonsági szempontból sérült rendszer automatikusan javítja.</span><span class="sxs-lookup"><span data-stu-id="b8078-156">For example, when hello user password is reset, many risk events that indicate that hello previous password was compromised are automatically remediated.</span></span>

### <a name="remediation"></a><span data-ttu-id="b8078-157">Szervizkiszolgáló</span><span class="sxs-lookup"><span data-stu-id="b8078-157">Remediation</span></span>
<span data-ttu-id="b8078-158">Egy művelet toosecure identitás vagy egy eszköz, amely korábban gyanús vagy toobe ismert biztonsági sérülés esetén.</span><span class="sxs-lookup"><span data-stu-id="b8078-158">An action toosecure an identity or a device that were previously suspected or known toobe compromised.</span></span> <span data-ttu-id="b8078-159">A javítási művelet visszaállítja a hello identitás vagy az eszköz tooa biztonságos állapotát, és oldja fel az előző kockázati események társított hello identitás vagy az eszköz.</span><span class="sxs-lookup"><span data-stu-id="b8078-159">A remediation action restores hello identity or device tooa safe state, and resolves previous risk events associated with hello identity or device.</span></span>

### <a name="resolved-risk-event"></a><span data-ttu-id="b8078-160">Megoldott (kockázat esemény)</span><span class="sxs-lookup"><span data-stu-id="b8078-160">Resolved (risk event)</span></span>
<span data-ttu-id="b8078-161">Állítsa be manuálisan egy Identity Protection felhasználónak, jelezve, hogy hello felhasználó elvégez egy megfelelő szervizelési művelet kívül Identity Protection, és adott hello kockázat eseményt kell kezelni a kockázat eseményállapot zárva.</span><span class="sxs-lookup"><span data-stu-id="b8078-161">A risk event status set manually by an Identity Protection user, indicating that hello user took an appropriate remediation action outside Identity Protection, and that hello risk event should be considered closed.</span></span>

### <a name="risk-event-status"></a><span data-ttu-id="b8078-162">Kockázati eseményállapot</span><span class="sxs-lookup"><span data-stu-id="b8078-162">Risk event status</span></span>
<span data-ttu-id="b8078-163">A kockázati események tulajdonságának, amely jelzi, hogy aktív-e hello esemény, és ha bezárul, bezárása, hello okát.</span><span class="sxs-lookup"><span data-stu-id="b8078-163">A property of a risk event, indicating whether hello event is active, and if closed, hello reason for closing it.</span></span>

### <a name="risk-event-type"></a><span data-ttu-id="b8078-164">Kockázati esemény típusa</span><span class="sxs-lookup"><span data-stu-id="b8078-164">Risk event type</span></span>
<span data-ttu-id="b8078-165">Hello kategóriáját eseményt, hello esemény toobe tekinthető kockázatos okozó anomáliadetektálási hello típusú kockázatát.</span><span class="sxs-lookup"><span data-stu-id="b8078-165">A category for hello risk event, indicating hello type of anomaly that caused hello event toobe considered risky.</span></span>

### <a name="risk-level-risk-event"></a><span data-ttu-id="b8078-166">Kockázati szint (kockázat esemény)</span><span class="sxs-lookup"><span data-stu-id="b8078-166">Risk level (risk event)</span></span>
<span data-ttu-id="b8078-167">Hello kockázat esemény toohelp Identity Protection felhasználók hello súlyossága megjelölése (magas, közepes vagy alacsony) rangsorolhatja hello műveletek tooreduce hello kockázati tootheir szervezet tartanak.</span><span class="sxs-lookup"><span data-stu-id="b8078-167">An indication (High, Medium, or Low) of hello severity of hello risk event toohelp Identity Protection users prioritize hello actions they take tooreduce hello risk tootheir organization.</span></span> 

### <a name="risk-level-sign-in"></a><span data-ttu-id="b8078-168">Kockázati szint (bejelentkezés)</span><span class="sxs-lookup"><span data-stu-id="b8078-168">Risk level (sign-in)</span></span>
<span data-ttu-id="b8078-169">Arra utal, hogy (magas, közepes vagy alacsony) hello valószínűségét, hogy az egy adott bejelentkezés, valaki más próbál toouse hello felhasználói azonosító.</span><span class="sxs-lookup"><span data-stu-id="b8078-169">An indication (High, Medium, or Low) of hello likelihood that for a specific sign-in, someone else is attempting toouse hello user’s identity.</span></span>

### <a name="risk-level-user-compromise"></a><span data-ttu-id="b8078-170">Kockázati szint (felhasználói sérült biztonság esetén)</span><span class="sxs-lookup"><span data-stu-id="b8078-170">Risk level (user compromise)</span></span>
<span data-ttu-id="b8078-171">Arra utal, hogy (magas, közepes vagy alacsony) hello valószínűségét, hogy identitás megtámadták.</span><span class="sxs-lookup"><span data-stu-id="b8078-171">An indication (High, Medium, or Low) of hello likelihood that an identity has been compromised.</span></span>

### <a name="risk-level-vulnerability"></a><span data-ttu-id="b8078-172">Kockázati szint (biztonsági rés)</span><span class="sxs-lookup"><span data-stu-id="b8078-172">Risk level (vulnerability)</span></span>
<span data-ttu-id="b8078-173">Hello biztonsági rés toohelp Identity Protection felhasználók hello súlyossága megjelölése (magas, közepes vagy alacsony) rangsorolhatja hello műveletek tooreduce hello kockázati tootheir szervezet tartanak.</span><span class="sxs-lookup"><span data-stu-id="b8078-173">An indication (High, Medium, or Low) of hello severity of hello vulnerability toohelp Identity Protection users prioritize hello actions they take tooreduce hello risk tootheir organization.</span></span>

### <a name="secure-identity"></a><span data-ttu-id="b8078-174">Biztonságos (identitás)</span><span class="sxs-lookup"><span data-stu-id="b8078-174">Secure (identity)</span></span>
<span data-ttu-id="b8078-175">Például a jelszó módosítása vagy a gép, különösen a potenciálisan veszélyeztetett identitás biztonsági szempontból sértetlen tooan állapot toorestore szervizelési művelet végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="b8078-175">Take remediation action such as a password change or machine reimaging toorestore a potentially compromised identity tooan uncompromised state.</span></span>

### <a name="security-policy"></a><span data-ttu-id="b8078-176">Biztonsági házirend</span><span class="sxs-lookup"><span data-stu-id="b8078-176">Security policy</span></span>
<span data-ttu-id="b8078-177">Szabályok és az állapot gyűjteménye.</span><span class="sxs-lookup"><span data-stu-id="b8078-177">A collection of policy rules and condition.</span></span> <span data-ttu-id="b8078-178">Egy házirend lehet például a felhasználók, csoportok, alkalmazások, eszközök, eszközök, eszköz állapotok, IP-címtartományok és Auth2.0 ügyféltípusokat alkalmazott tooentities.</span><span class="sxs-lookup"><span data-stu-id="b8078-178">A policy can be applied tooentities such as users, groups, apps, devices, device platforms, device states, IP ranges, and Auth2.0 client types.</span></span> <span data-ttu-id="b8078-179">A házirend engedélyezve van, amikor egy entitás hello házirendben erőforrás jogkivonatot ad ki lesznek kiértékelve.</span><span class="sxs-lookup"><span data-stu-id="b8078-179">When a policy is enabled, it is evaluated whenever an entity included in hello policy is issued a token for a resource.</span></span>

### <a name="sign-in-v"></a><span data-ttu-id="b8078-180">Jelentkezzen be (v)</span><span class="sxs-lookup"><span data-stu-id="b8078-180">Sign in (v)</span></span>
<span data-ttu-id="b8078-181">tooauthenticate tooan identitás, az Azure Active Directoryban.</span><span class="sxs-lookup"><span data-stu-id="b8078-181">tooauthenticate tooan identity in Azure Active Directory.</span></span>

### <a name="sign-in-n"></a><span data-ttu-id="b8078-182">Bejelentkezés (n)</span><span class="sxs-lookup"><span data-stu-id="b8078-182">Sign-in (n)</span></span>
<span data-ttu-id="b8078-183">hello folyamat vagy egy Azure Active Directory és a hello esemény, amely a művelet rögzíti a személyazonossága hitelesítésének művelettel.</span><span class="sxs-lookup"><span data-stu-id="b8078-183">hello process or action of authenticating an identity in Azure Active Directory, and hello event that captures this operation.</span></span>

### <a name="sign-in-from-anonymous-ip-address"></a><span data-ttu-id="b8078-184">Bejelentkezés a névtelen IP-cím</span><span class="sxs-lookup"><span data-stu-id="b8078-184">Sign-in from anonymous IP address</span></span>
<span data-ttu-id="b8078-185">A kockázat esemény után egy sikeres bejelentkezés névtelen proxy IP-címként azonosított IP-címről következik be.</span><span class="sxs-lookup"><span data-stu-id="b8078-185">A risk event triggered after a successful sign-in from IP address that has been identified as an anonymous proxy IP address.</span></span>

### <a name="sign-in-from-infected-device"></a><span data-ttu-id="b8078-186">Bejelentkezés a fertőzött eszköz</span><span class="sxs-lookup"><span data-stu-id="b8078-186">Sign-in from infected device</span></span>
<span data-ttu-id="b8078-187">A kockázat a bejelentkezés más néven toobe egy vagy több feltört eszközök, amelyek aktívan próbált botot kiszolgálóval toocommunicate által használt IP-cím származik által elindított esemény.</span><span class="sxs-lookup"><span data-stu-id="b8078-187">A risk event triggered when a sign-in originates from an IP address which is known toobe used by one or more compromised devices, which are actively attempting toocommunicate with a bot server.</span></span>

### <a name="sign-in-from-ip-address-with-suspicious-activity"></a><span data-ttu-id="b8078-188">Jelentkezzen be a következő IP-gyanús tevékenység</span><span class="sxs-lookup"><span data-stu-id="b8078-188">Sign-in from IP address with suspicious activity</span></span>
<span data-ttu-id="b8078-189">A kockázat az esemény akkor váltódik ki, miután egy sikeres bejelentkezés IP cím nagyszámú sikertelen bejelentkezési kísérlet között több felhasználói fiókot egy rövid időtartamra vonatkozóan.</span><span class="sxs-lookup"><span data-stu-id="b8078-189">A risk event triggered after a successful sign-in from an IP address with a high number of failed login attempts across multiple user accounts over a short period of time.</span></span>

### <a name="sign-in-from-unfamiliar-location"></a><span data-ttu-id="b8078-190">Bejelentkezés ismeretlen helyről</span><span class="sxs-lookup"><span data-stu-id="b8078-190">Sign-in from unfamiliar location</span></span>
<span data-ttu-id="b8078-191">A kockázat felhasználó sikeresen jelentkezik be egy új helyről (IP, szélesség/hosszúsági és ASN) által elindított esemény.</span><span class="sxs-lookup"><span data-stu-id="b8078-191">A risk event triggered when a user successfully signs in from a new location (IP, Latitude/Longitude and ASN).</span></span>

### <a name="sign-in-risk"></a><span data-ttu-id="b8078-192">Bejelentkezési kockázata</span><span class="sxs-lookup"><span data-stu-id="b8078-192">Sign-in risk</span></span>
<span data-ttu-id="b8078-193">Tekintse meg a kockázati szintjét (bejelentkezés)</span><span class="sxs-lookup"><span data-stu-id="b8078-193">See Risk level (sign-in)</span></span>

### <a name="sign-in-risk-policy"></a><span data-ttu-id="b8078-194">Bejelentkezési kockázat házirendnek</span><span class="sxs-lookup"><span data-stu-id="b8078-194">Sign-in risk policy</span></span>
<span data-ttu-id="b8078-195">Feltételes hozzáférési szabályzatot, amely hello kockázati tooa adott bejelentkezés és azok mérséklési előre meghatározott feltételt és a szabályok alapján alkalmazza.</span><span class="sxs-lookup"><span data-stu-id="b8078-195">A conditional access policy that evaluates hello risk tooa specific sign-in and applies mitigations based on predefined conditions and rules.</span></span>

### <a name="user-compromise-risk"></a><span data-ttu-id="b8078-196">Felhasználó biztonsági sérülés kockázata</span><span class="sxs-lookup"><span data-stu-id="b8078-196">User compromise risk</span></span>
<span data-ttu-id="b8078-197">Tekintse meg a kockázati szintjét (felhasználói sérült biztonság esetén)</span><span class="sxs-lookup"><span data-stu-id="b8078-197">See Risk level (user compromise)</span></span>

### <a name="user-risk"></a><span data-ttu-id="b8078-198">Felhasználói kockázata</span><span class="sxs-lookup"><span data-stu-id="b8078-198">User risk</span></span>
<span data-ttu-id="b8078-199">Tekintse meg a kockázati szintjét (felhasználói sérült biztonság esetén).</span><span class="sxs-lookup"><span data-stu-id="b8078-199">See Risk level (user compromise).</span></span>

### <a name="user-risk-policy"></a><span data-ttu-id="b8078-200">Felhasználói kockázat házirendnek</span><span class="sxs-lookup"><span data-stu-id="b8078-200">User risk policy</span></span>
<span data-ttu-id="b8078-201">Feltételes hozzáférési szabályzatot, amely hello bejelentkezés úgy ítéli meg, és azok mérséklési előre meghatározott feltételt és a szabályok alapján alkalmazza.</span><span class="sxs-lookup"><span data-stu-id="b8078-201">A conditional access policy that considers hello sign-in and applies mitigations based on predefined conditions and rules.</span></span>

### <a name="users-flagged-for-risk"></a><span data-ttu-id="b8078-202">Kockázatosként megjelölt felhasználók</span><span class="sxs-lookup"><span data-stu-id="b8078-202">Users flagged for risk</span></span>
<span data-ttu-id="b8078-203">Kockázati eseményekről, amelyek aktív vagy szervizelt rendelkező felhasználók</span><span class="sxs-lookup"><span data-stu-id="b8078-203">Users that have risk events which are either active or remediated</span></span>

### <a name="vulnerability"></a><span data-ttu-id="b8078-204">A biztonsági rés</span><span class="sxs-lookup"><span data-stu-id="b8078-204">Vulnerability</span></span>
<span data-ttu-id="b8078-205">Egy konfigurációs vagy az Azure Active Directoryban, így hello directory ki vannak téve tooexploits vagy fenyegetések feltétel.</span><span class="sxs-lookup"><span data-stu-id="b8078-205">A configuration or condition in Azure Active Directory which makes hello directory susceptible tooexploits or threats.</span></span>

## <a name="see-also"></a><span data-ttu-id="b8078-206">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="b8078-206">See also</span></span>
* [<span data-ttu-id="b8078-207">Az Azure Active Directory azonosító adatok védelmét</span><span class="sxs-lookup"><span data-stu-id="b8078-207">Azure Active Directory Identity Protection</span></span>](active-directory-identityprotection.md)

