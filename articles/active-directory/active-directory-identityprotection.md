---
title: Active Directory-Identity Protection aaaAzure |} Microsoft Docs
description: "Ismerje meg, az Azure AD Identity Protection miként toolimit hello azon képessége, egy támadó tooexploit egy sérült biztonságú identitás vagy az eszköz és a toosecure identitás vagy egy eszköz, amely korábban gyanús vagy ismert toobe sérült."
services: active-directory
keywords: "az Azure active directory azonosító adatok védelmét, a cloud app discovery, alkalmazások, biztonság, kockázat, kockázati szint, biztonsági rés, biztonsági házirend kezelése"
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: e7434eeb-4e98-4b6b-a895-b5598a6cccf1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: ecca4f3cdb65585687cf44a80024f26c7cab22ca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-identity-protection"></a><span data-ttu-id="bfaac-104">Azure Active Directory Identity Protection</span><span class="sxs-lookup"><span data-stu-id="bfaac-104">Azure Active Directory Identity Protection</span></span>

<span data-ttu-id="bfaac-105">Az Azure Active Directory azonosító adatok védelmét lehetővé teszi az Azure AD Premium P2 hello kiadás, amely lehetővé teszi:</span><span class="sxs-lookup"><span data-stu-id="bfaac-105">Azure Active Directory Identity Protection is a feature of hello Azure AD Premium P2 edition that enables you to:</span></span>

- <span data-ttu-id="bfaac-106">A szervezet identitásait érintő lehetséges biztonsági rések észlelése</span><span class="sxs-lookup"><span data-stu-id="bfaac-106">Detect potential vulnerabilities affecting your organization’s identities</span></span>

- <span data-ttu-id="bfaac-107">Automatikus válaszok toodetected gyanús műveleteket kapcsolódó tooyour szervezet identitásait konfigurálása</span><span class="sxs-lookup"><span data-stu-id="bfaac-107">Configure automated responses toodetected suspicious actions that are related tooyour organization’s identities</span></span>  

- <span data-ttu-id="bfaac-108">Vizsgálja meg a gyanús incidensek, és tegye meg a megfelelő lépéseket tooresolve őket</span><span class="sxs-lookup"><span data-stu-id="bfaac-108">Investigate suspicious incidents and take appropriate action tooresolve them</span></span>   


## <a name="getting-started"></a><span data-ttu-id="bfaac-109">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="bfaac-109">Getting started</span></span>

<span data-ttu-id="bfaac-110">A Microsoft felhőalapú Identitások a több mint egy évtizedben biztonságossá tételére.</span><span class="sxs-lookup"><span data-stu-id="bfaac-110">Microsoft secures cloud-based identities for more than a decade.</span></span> <span data-ttu-id="bfaac-111">Azure Active Directory azonosító adatok védelmét, a környezetben, segítségével hello azonos védelmi rendszerek Microsoft toosecure identitások használja.</span><span class="sxs-lookup"><span data-stu-id="bfaac-111">With Azure Active Directory Identity Protection, in your environment, you can use hello same protection systems Microsoft uses toosecure identities.</span></span>

<span data-ttu-id="bfaac-112">hello többsége megszegése érvénybe biztonsági sor, amikor a támadók hozzáférést tooan környezet nyerhet ellophatják a felhasználó identitását.</span><span class="sxs-lookup"><span data-stu-id="bfaac-112">hello vast majority of security breaches take place when attackers gain access tooan environment by stealing a user’s identity.</span></span> <span data-ttu-id="bfaac-113">Hello évek támadók egyre hatékony, a harmadik féltől származó megszegése követését és kifinomult adathalász támadások használatával lettek.</span><span class="sxs-lookup"><span data-stu-id="bfaac-113">Over hello years, attackers have become increasingly effective in leveraging third party breaches and using sophisticated phishing attacks.</span></span> <span data-ttu-id="bfaac-114">Amint egy támadó hozzáfér tooeven alacsony szintű jogosultságokkal rendelkező felhasználói fiókok, is viszonylag egyszerűen toogain tooimportant vállalati erőforrások eléréséhez oldalirányú mozgás révén azokat.</span><span class="sxs-lookup"><span data-stu-id="bfaac-114">As soon as an attacker gains access tooeven low privileged user accounts, it is relatively easy for them toogain access tooimportant company resources through lateral movement.</span></span>

<span data-ttu-id="bfaac-115">Ennek következtében kell:</span><span class="sxs-lookup"><span data-stu-id="bfaac-115">As a consequence of this, you need to:</span></span>

- <span data-ttu-id="bfaac-116">A jogosultsági szint függetlenül minden identitások védelme</span><span class="sxs-lookup"><span data-stu-id="bfaac-116">Protect all identities regardless of their privilege level</span></span>

- <span data-ttu-id="bfaac-117">Proaktív a sérült biztonságú identitások megakadályozása visszaélt folyamatban</span><span class="sxs-lookup"><span data-stu-id="bfaac-117">Proactively prevent compromised identities from being abused</span></span>

<span data-ttu-id="bfaac-118">Sérült biztonságú identitások felderítésére nem egyszerű feladat.</span><span class="sxs-lookup"><span data-stu-id="bfaac-118">Discovering compromised identities is no easy task.</span></span> <span data-ttu-id="bfaac-119">Az Azure Active Directory adaptív gépi tanulási algoritmusok és heurisztikus toodetect rendellenességek észlelését és gyanús incidenseket, amelyek jelzik, potenciálisan sérült identitások szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="bfaac-119">Azure Active Directory uses adaptive machine learning algorithms and heuristics toodetect anomalies and suspicious incidents that indicate potentially compromised identities.</span></span> <span data-ttu-id="bfaac-120">Ezeket az adatokat használva Identity Protection létrehozza a jelentéseket és riasztásokat, amelyek lehetővé teszik a tooevaluate hello problémákat észlelt, és megfelelő megoldás vagy elvégzett szervizelési műveleteket.</span><span class="sxs-lookup"><span data-stu-id="bfaac-120">Using this data, Identity Protection generates reports and alerts that enable you tooevaluate hello detected issues and take appropriate mitigation or remediation actions.</span></span>

<span data-ttu-id="bfaac-121">Az Azure Active Directory azonosító adatok védelmét a rendszer több mint egy figyelési és jelentéskészítési eszköz.</span><span class="sxs-lookup"><span data-stu-id="bfaac-121">Azure Active Directory Identity Protection is more than a monitoring and reporting tool.</span></span> <span data-ttu-id="bfaac-122">tooprotect a szervezet identitásait, kockázati-alapú, toodetected problémákat automatikusan válaszol, amikor a rendszer elérte a megadott kockázati szint házirendeket konfigurálhat.</span><span class="sxs-lookup"><span data-stu-id="bfaac-122">tooprotect your organization's identities, you can configure risk-based policies that automatically respond toodetected issues when a specified risk level has been reached.</span></span> <span data-ttu-id="bfaac-123">Ezek a házirendek továbbá tooother feltételes hozzáférés-szabályozási Azure Active Directory és az EMS biztosítja, vagy automatikusan letilthatja vagy kezdeményezheti, többek között a jelszó alaphelyzetbe állítását és a többtényezős hitelesítés kényszerítése adaptív elvégzett szervizelési műveleteket.</span><span class="sxs-lookup"><span data-stu-id="bfaac-123">These policies, in addition tooother conditional access controls provided by Azure Active Directory and EMS, can either automatically block or initiate adaptive remediation actions including password resets and multi-factor authentication enforcement.</span></span>


#### <a name="identity-protection-capabilities"></a><span data-ttu-id="bfaac-124">Identitás-megelőzési képességek</span><span class="sxs-lookup"><span data-stu-id="bfaac-124">Identity Protection capabilities</span></span>

<span data-ttu-id="bfaac-125">**Biztonsági rések és kockázatos fiókok észlelése:**</span><span class="sxs-lookup"><span data-stu-id="bfaac-125">**Detecting vulnerabilities and risky accounts:**</span></span>  

* <span data-ttu-id="bfaac-126">Egyéni javaslatok tooimprove nyújtó általános biztonsági állapotát a biztonsági rések kiemelésével</span><span class="sxs-lookup"><span data-stu-id="bfaac-126">Providing custom recommendations tooimprove overall security posture by highlighting vulnerabilities</span></span>
* <span data-ttu-id="bfaac-127">Bejelentkezési kockázati szintek kiszámítása</span><span class="sxs-lookup"><span data-stu-id="bfaac-127">Calculating sign-in risk levels</span></span>
* <span data-ttu-id="bfaac-128">Kockázati szintek felhasználói kiszámítása</span><span class="sxs-lookup"><span data-stu-id="bfaac-128">Calculating user risk levels</span></span>


<span data-ttu-id="bfaac-129">**Vizsgáló kockázati események:**</span><span class="sxs-lookup"><span data-stu-id="bfaac-129">**Investigating risk events:**</span></span>

* <span data-ttu-id="bfaac-130">Értesítésküldési kockázati események</span><span class="sxs-lookup"><span data-stu-id="bfaac-130">Sending notifications for risk events</span></span>
* <span data-ttu-id="bfaac-131">Megfelelő és környezetfüggő információk alapján kockázati események kivizsgálása</span><span class="sxs-lookup"><span data-stu-id="bfaac-131">Investigating risk events using relevant and contextual information</span></span>
* <span data-ttu-id="bfaac-132">A munkafolyamatok alapvető tootrack vizsgálatok biztosítása</span><span class="sxs-lookup"><span data-stu-id="bfaac-132">Providing basic workflows tootrack investigations</span></span>
* <span data-ttu-id="bfaac-133">Így könnyen elérhetők tooremediation műveletek, például a jelszó alaphelyzetbe állítása</span><span class="sxs-lookup"><span data-stu-id="bfaac-133">Providing easy access tooremediation actions such as password reset</span></span>

<span data-ttu-id="bfaac-134">**Kockázati-alapú feltételes hozzáférési házirendek:**</span><span class="sxs-lookup"><span data-stu-id="bfaac-134">**Risk-based conditional access policies:**</span></span>

* <span data-ttu-id="bfaac-135">Házirend toomitigate kockázatos bejelentkezések bejelentkezések blokkolja, vagy a multi-factor authentication kihívások szükségessé tételével.</span><span class="sxs-lookup"><span data-stu-id="bfaac-135">Policy toomitigate risky sign-ins by blocking sign-ins or requiring multi-factor authentication challenges.</span></span>
* <span data-ttu-id="bfaac-136">Házirend tooblock vagy biztonságos kockázatos felhasználói fiókok</span><span class="sxs-lookup"><span data-stu-id="bfaac-136">Policy tooblock or secure risky user accounts</span></span>
* <span data-ttu-id="bfaac-137">Házirend toorequire felhasználók tooregister a többtényezős hitelesítés</span><span class="sxs-lookup"><span data-stu-id="bfaac-137">Policy toorequire users tooregister for multi-factor authentication</span></span>



## <a name="identity-protection-roles"></a><span data-ttu-id="bfaac-138">Identity Protection szerepkörök</span><span class="sxs-lookup"><span data-stu-id="bfaac-138">Identity Protection roles</span></span>

<span data-ttu-id="bfaac-139">tooload egyenleg hello felügyeleti tevékenységek Identity Protection implementálásának körül, több szerepköröket rendelhet.</span><span class="sxs-lookup"><span data-stu-id="bfaac-139">tooload balance hello management activities around your Identity Protection implementation, you can assign several roles.</span></span> <span data-ttu-id="bfaac-140">Az Azure AD Identity Protection 3 directory szerepkörök támogatja:</span><span class="sxs-lookup"><span data-stu-id="bfaac-140">Azure AD Identity Protection supports 3 directory roles:</span></span>

| <span data-ttu-id="bfaac-141">Szerepkör</span><span class="sxs-lookup"><span data-stu-id="bfaac-141">Role</span></span>                         | <span data-ttu-id="bfaac-142">Teheti meg</span><span class="sxs-lookup"><span data-stu-id="bfaac-142">Can do</span></span>                          | <span data-ttu-id="bfaac-143">Nem hajtható végre</span><span class="sxs-lookup"><span data-stu-id="bfaac-143">Cannot do</span></span>
| :--                          | ---                                |  ---   |
| <span data-ttu-id="bfaac-144">Globális rendszergazda</span><span class="sxs-lookup"><span data-stu-id="bfaac-144">Global administrator</span></span>         | <span data-ttu-id="bfaac-145">Teljes hozzáférés tooIdentity védelmi, bevezetni, Identity Protection</span><span class="sxs-lookup"><span data-stu-id="bfaac-145">Full access tooIdentity Protection, Onboard Identity Protection</span></span>| |
| <span data-ttu-id="bfaac-146">Biztonsági rendszergazda</span><span class="sxs-lookup"><span data-stu-id="bfaac-146">Security administrator</span></span>       | <span data-ttu-id="bfaac-147">Teljes hozzáférés tooIdentity védelme</span><span class="sxs-lookup"><span data-stu-id="bfaac-147">Full access tooIdentity Protection</span></span> | <span data-ttu-id="bfaac-148">Identity Protection előkészítésére, állítsa vissza egy felhasználó jelszavát</span><span class="sxs-lookup"><span data-stu-id="bfaac-148">Onboard Identity Protection,  reset passwords for a user</span></span> |
| <span data-ttu-id="bfaac-149">Biztonsági olvasó</span><span class="sxs-lookup"><span data-stu-id="bfaac-149">Security reader</span></span>              | <span data-ttu-id="bfaac-150">Csak kész tooIdentity védelme</span><span class="sxs-lookup"><span data-stu-id="bfaac-150">Ready-only access tooIdentity Protection</span></span> | <span data-ttu-id="bfaac-151">A bevezetni, Identity Protection, remidiate felhasználók, házirendeket konfigurálhat, jelszavak alaphelyzetbe állítása</span><span class="sxs-lookup"><span data-stu-id="bfaac-151">Onboard Identity Protection, remidiate users, configure policies,  reset passwords</span></span> |




<span data-ttu-id="bfaac-152">További részletekért lásd: [rendszergazdai szerepkörök hozzárendelése az Azure Active Directoryban](active-directory-assign-admin-roles-azure-portal.md)</span><span class="sxs-lookup"><span data-stu-id="bfaac-152">For more details, see [Assigning administrator roles in Azure Active Directory](active-directory-assign-admin-roles-azure-portal.md)</span></span>



## <a name="detection"></a><span data-ttu-id="bfaac-153">Észlelés</span><span class="sxs-lookup"><span data-stu-id="bfaac-153">Detection</span></span>

### <a name="vulnerabilities"></a><span data-ttu-id="bfaac-154">Biztonsági rések</span><span class="sxs-lookup"><span data-stu-id="bfaac-154">Vulnerabilities</span></span>

<span data-ttu-id="bfaac-155">Az Azure Active Directory azonosító adatok védelmét a konfigurációs és a biztonsági réseket, amelyek hatással lehetnek a felhasználói identitások észleli.</span><span class="sxs-lookup"><span data-stu-id="bfaac-155">Azure Active Directory Identity Protection analyses your configuration and detects vulnerabilities that can have an impact on your user's identities.</span></span> <span data-ttu-id="bfaac-156">További részletekért lásd: [Azure Active Directory Identity Protection által észlelt biztonsági rések](active-directory-identityprotection-vulnerabilities.md).</span><span class="sxs-lookup"><span data-stu-id="bfaac-156">For more details, see [Vulnerabilities detected by Azure Active Directory Identity Protection](active-directory-identityprotection-vulnerabilities.md).</span></span>

### <a name="risk-events"></a><span data-ttu-id="bfaac-157">Kockázati események</span><span class="sxs-lookup"><span data-stu-id="bfaac-157">Risk events</span></span>

<span data-ttu-id="bfaac-158">Az Azure Active Directory adaptív gépi tanulási a algoritmusok és heurisztikus toodetect gyanús műveleteket tooyour kapcsolódó felhasználói identitások használja.</span><span class="sxs-lookup"><span data-stu-id="bfaac-158">Azure Active Directory uses adaptive machine learning algorithms and heuristics toodetect suspicious actions that are related tooyour user's identities.</span></span> <span data-ttu-id="bfaac-159">hello rendszer létrehoz egy rekordot észlelt gyanús műveleteket.</span><span class="sxs-lookup"><span data-stu-id="bfaac-159">hello system creates a record for each detected suspicious action.</span></span> <span data-ttu-id="bfaac-160">Ezeket a rekordokat kockázati események is ismertek.</span><span class="sxs-lookup"><span data-stu-id="bfaac-160">These records are also known as risk events.</span></span>  
<span data-ttu-id="bfaac-161">További részletek: [Az Azure Active Directory kockázati eseményei](active-directory-identity-protection-risk-events.md).</span><span class="sxs-lookup"><span data-stu-id="bfaac-161">For more details, see [Azure Active Directory risk events](active-directory-identity-protection-risk-events.md).</span></span>


## <a name="investigation"></a><span data-ttu-id="bfaac-162">Vizsgálat</span><span class="sxs-lookup"><span data-stu-id="bfaac-162">Investigation</span></span>
<span data-ttu-id="bfaac-163">A használatában Identity Protection keresztül általában hello Identity Protection-irányítópult kezdődik.</span><span class="sxs-lookup"><span data-stu-id="bfaac-163">Your journey through Identity Protection typically starts with hello Identity Protection dashboard.</span></span>

<span data-ttu-id="bfaac-164">![Szervizelés](./media/active-directory-identityprotection/1000.png "szervizelés")</span><span class="sxs-lookup"><span data-stu-id="bfaac-164">![Remediation](./media/active-directory-identityprotection/1000.png "Remediation")</span></span>

<span data-ttu-id="bfaac-165">hello irányítópult a következő hozzáférési jogosultságot a következőhöz:</span><span class="sxs-lookup"><span data-stu-id="bfaac-165">hello dashboard gives you access to:</span></span>

* <span data-ttu-id="bfaac-166">Például a jelentések **felhasználók meg van jelölve, a kockázat**, **kockázati események** és **biztonsági réseket**</span><span class="sxs-lookup"><span data-stu-id="bfaac-166">Reports such as **Users flagged for risk**, **Risk events** and **Vulnerabilities**</span></span>
* <span data-ttu-id="bfaac-167">Beállítások, például hello konfigurálása a **biztonsági házirendek**, **értesítések** és **a multi-factor authentication regisztráció**</span><span class="sxs-lookup"><span data-stu-id="bfaac-167">Settings such as hello configuration of your **Security Policies**, **Notifications** and **multi-factor authentication registration**</span></span>

<span data-ttu-id="bfaac-168">Általában le a vizsgálat, amely hello tevékenységek, a naplókat, és olyan releváns információkat kapcsolódó tooa kockázati esemény toodecide szervizelési vagy enyhítési lépések szükségesek, hogy véleményezési hello eljárás, és milyen hello identitás volt a kiindulási pont sérült, és megérteni, hogyan hello sérült azonosító lett megadva.</span><span class="sxs-lookup"><span data-stu-id="bfaac-168">It is typically your starting point for investigation, which is hello process of reviewing hello activities, logs, and other relevant information related tooa risk event toodecide whether remediation or mitigation steps are necessary,  and how hello identity was compromised, and understand how hello compromised identity was used.</span></span>

<span data-ttu-id="bfaac-169">Ön nagy terhelést jelent a vizsgálat tevékenységek toohello [értesítések](active-directory-identityprotection-notifications.md) Azure Active Directory Protection küld egy e-mailt.</span><span class="sxs-lookup"><span data-stu-id="bfaac-169">You can tie your investigation activities toohello [notifications](active-directory-identityprotection-notifications.md) Azure Active Directory Protection sends per email.</span></span>

<span data-ttu-id="bfaac-170">hello alább láthatja a további részletek és kapcsolódó tooan vizsgálat hello lépéseket.</span><span class="sxs-lookup"><span data-stu-id="bfaac-170">hello following sections provide you with more details and hello steps that are related tooan investigation.</span></span>  


## <a name="risky-sign-ins"></a><span data-ttu-id="bfaac-171">Kockázatos bejelentkezések</span><span class="sxs-lookup"><span data-stu-id="bfaac-171">Risky sign-ins</span></span>

<span data-ttu-id="bfaac-172">Az Azure Active Directory észleli [eseménytípusok kockázati](active-directory-reporting-risk-events.md#risk-event-types) valós idejű és offline állapotban.</span><span class="sxs-lookup"><span data-stu-id="bfaac-172">Azure Active Directory detects [risk event types](active-directory-reporting-risk-events.md#risk-event-types) in real-time and offline.</span></span> <span data-ttu-id="bfaac-173">Minden kockázati esemény észlelhető a bejelentkezés felhasználói a(z) hozzájárul tooa kockázatos bejelentkezés nevű logikai fogalom.</span><span class="sxs-lookup"><span data-stu-id="bfaac-173">Each risk event that has been detected for a sign-in of a user contributes tooa logical concept called risky sign-in.</span></span> <span data-ttu-id="bfaac-174">Egy kockázatos bejelentkezés egy bejelentkezési kísérlet után előfordulhat, hogy nem elvégzett hello jogos tulajdonos egy felhasználói fiók mutatója.</span><span class="sxs-lookup"><span data-stu-id="bfaac-174">A risky sign-in is an indicator for a sign-in attempt that might not have been performed by hello legitimate owner of a user account.</span></span>


### <a name="sign-in-risk-level"></a><span data-ttu-id="bfaac-175">Bejelentkezési kockázati szint</span><span class="sxs-lookup"><span data-stu-id="bfaac-175">Sign-in risk level</span></span>

<span data-ttu-id="bfaac-176">A bejelentkezési kockázati szintje feltüntetése (magas, közepes vagy alacsony) hello valószínűsége, hogy egy bejelentkezési kísérlet után nem lett végrehajtva hello jogos tulajdonos egy felhasználói fiók.</span><span class="sxs-lookup"><span data-stu-id="bfaac-176">A sign-in risk level is an indication (High, Medium, or Low) of hello likelihood that a sign-in attempt was not performed by hello legitimate owner of a user account.</span></span>

### <a name="mitigating-sign-in-risk-events"></a><span data-ttu-id="bfaac-177">Bejelentkezési kockázati események orvoslása</span><span class="sxs-lookup"><span data-stu-id="bfaac-177">Mitigating sign-in risk events</span></span>

<span data-ttu-id="bfaac-178">A megoldás egy művelet toolimit hello azon képessége, egy támadó tooexploit egy sérült biztonságú identitás vagy az eszköz hello identitás vagy az eszköz tooa biztonságos állapot visszaállítása nélkül.</span><span class="sxs-lookup"><span data-stu-id="bfaac-178">A mitigation is an action toolimit hello ability of an attacker tooexploit a compromised identity or device without restoring hello identity or device tooa safe state.</span></span> <span data-ttu-id="bfaac-179">A megoldás nem oldja meg a társított hello identitás vagy az eszköz korábbi bejelentkezési kockázati eseményekről.</span><span class="sxs-lookup"><span data-stu-id="bfaac-179">A mitigation does not resolve previous sign-in risk events associated with hello identity or device.</span></span>

<span data-ttu-id="bfaac-180">kockázatos toomitigate bejelentkezéseket automatikusan, konfigurálhatja a bejelentkezési kockázat biztonsági policicies.</span><span class="sxs-lookup"><span data-stu-id="bfaac-180">toomitigate risky sign-ins automatically, you can configure sign-in risk security policicies.</span></span> <span data-ttu-id="bfaac-181">Ezek a házirendek célszerű hello kockázati szintjét hello felhasználói vagy hello bejelentkezési tooblock kockázatos bejelentkezéseket vagy hello felhasználói tooperform többtényezős hitelesítést.</span><span class="sxs-lookup"><span data-stu-id="bfaac-181">Using these policies, you consider hello risk level of hello user or hello sign-in tooblock risky sign-ins or require hello user tooperform multi-factor authentication.</span></span> <span data-ttu-id="bfaac-182">Ezek a műveletek előfordulhat, hogy egy támadó a kihasználhassa egy ellopott identitás toocause kárt, és előfordulhat, hogy adjon bizonyos idő toosecure hello identitás.</span><span class="sxs-lookup"><span data-stu-id="bfaac-182">These actions may prevent an attacker from exploiting a stolen identity toocause damage, and may give you some time toosecure hello identity.</span></span>

### <a name="sign-in-risk-security-policy"></a><span data-ttu-id="bfaac-183">Bejelentkezési kockázat biztonsági házirend</span><span class="sxs-lookup"><span data-stu-id="bfaac-183">Sign-in risk security policy</span></span>
<span data-ttu-id="bfaac-184">A kockázat bejelentkezési házirend egy feltételes hozzáférési szabályzatot, amely hello kockázati tooa adott bejelentkezés és azok mérséklési előre meghatározott feltételt és a szabályok alapján alkalmazza.</span><span class="sxs-lookup"><span data-stu-id="bfaac-184">A sign-in risk policy is a conditional access policy that evaluates hello risk tooa specific sign-in and applies mitigations based on predefined conditions and rules.</span></span>

<span data-ttu-id="bfaac-185">![Bejelentkezési kockázat házirendnek](./media/active-directory-identityprotection/1014.png "bejelentkezési kockázat házirendnek")</span><span class="sxs-lookup"><span data-stu-id="bfaac-185">![Sign-in risk policy](./media/active-directory-identityprotection/1014.png "Sign-in risk policy")</span></span>

<span data-ttu-id="bfaac-186">Az Azure AD Identity Protection segítségével felügyelheti a hello megoldás a kockázatos bejelentkezések azáltal, hogy:</span><span class="sxs-lookup"><span data-stu-id="bfaac-186">Azure AD Identity Protection helps you manage hello mitigation of risky sign-ins by enabling you to:</span></span>

* <span data-ttu-id="bfaac-187">Állítsa be a felhasználók és csoportok hello házirend vonatkozik hello:</span><span class="sxs-lookup"><span data-stu-id="bfaac-187">Set hello users and groups hello policy applies to:</span></span>

    <span data-ttu-id="bfaac-188">![Bejelentkezési kockázat házirendnek](./media/active-directory-identityprotection/1015.png "bejelentkezési kockázat házirendnek")</span><span class="sxs-lookup"><span data-stu-id="bfaac-188">![Sign-in risk policy](./media/active-directory-identityprotection/1015.png "Sign-in risk policy")</span></span>
* <span data-ttu-id="bfaac-189">Hello bejelentkezési kockázati szint küszöbértéke (alacsony, közepes vagy magas), amely elindítja a hello házirend beállítása:</span><span class="sxs-lookup"><span data-stu-id="bfaac-189">Set hello sign-in risk level threshold (low, medium, or high) that triggers hello policy:</span></span>

    <span data-ttu-id="bfaac-190">![Bejelentkezési kockázat házirendnek](./media/active-directory-identityprotection/1016.png "bejelentkezési kockázat házirendnek")</span><span class="sxs-lookup"><span data-stu-id="bfaac-190">![Sign-in risk policy](./media/active-directory-identityprotection/1016.png "Sign-in risk policy")</span></span>
* <span data-ttu-id="bfaac-191">Set hello vezérlők toobe lépnek érvénybe, ha elindítja a hello házirend:</span><span class="sxs-lookup"><span data-stu-id="bfaac-191">Set hello controls toobe enforced when hello policy triggers:</span></span>  

    <span data-ttu-id="bfaac-192">![Bejelentkezési kockázat házirendnek](./media/active-directory-identityprotection/1017.png "bejelentkezési kockázat házirendnek")</span><span class="sxs-lookup"><span data-stu-id="bfaac-192">![Sign-in risk policy](./media/active-directory-identityprotection/1017.png "Sign-in risk policy")</span></span>
* <span data-ttu-id="bfaac-193">A házirend kapcsoló hello állapotát:</span><span class="sxs-lookup"><span data-stu-id="bfaac-193">Switch hello state of your policy:</span></span>

    <span data-ttu-id="bfaac-194">![Többtényezős hitelesítés regisztrációs](./media/active-directory-identityprotection/403.png "MFA-regisztráció")</span><span class="sxs-lookup"><span data-stu-id="bfaac-194">![MFA Registration](./media/active-directory-identityprotection/403.png "MFA Registration")</span></span>
* <span data-ttu-id="bfaac-195">Ellenőrizze, és értékelje hello hatással bír a változtatás aktiválás előtt:</span><span class="sxs-lookup"><span data-stu-id="bfaac-195">Review and evaluate hello impact of a change before activating it:</span></span>

    <span data-ttu-id="bfaac-196">![Bejelentkezési kockázat házirendnek](./media/active-directory-identityprotection/1018.png "bejelentkezési kockázat házirendnek")</span><span class="sxs-lookup"><span data-stu-id="bfaac-196">![Sign-in risk policy](./media/active-directory-identityprotection/1018.png "Sign-in risk policy")</span></span>

#### <a name="what-you-need-tooknow"></a><span data-ttu-id="bfaac-197">Mit kell tooknow</span><span class="sxs-lookup"><span data-stu-id="bfaac-197">What you need tooknow</span></span>
<span data-ttu-id="bfaac-198">Konfigurálhatja a bejelentkezési kockázat biztonsági házirend toorequire multi-factor Authentication hitelesítést:</span><span class="sxs-lookup"><span data-stu-id="bfaac-198">You can configure a sign-in risk security policy toorequire multi-factor authentication:</span></span>

<span data-ttu-id="bfaac-199">![Bejelentkezési kockázat házirendnek](./media/active-directory-identityprotection/1017.png "bejelentkezési kockázat házirendnek")</span><span class="sxs-lookup"><span data-stu-id="bfaac-199">![Sign-in risk policy](./media/active-directory-identityprotection/1017.png "Sign-in risk policy")</span></span>

<span data-ttu-id="bfaac-200">Azonban biztonsági okok miatt ez a beállítás csak akkor működik a felhasználók a multi-factor authentication már regisztrált.</span><span class="sxs-lookup"><span data-stu-id="bfaac-200">However, for security reasons, this setting only works for users that have already been registered for multi-factor authentication.</span></span> <span data-ttu-id="bfaac-201">Ha egy felhasználóhoz, aki még nincs regisztrálva a többtényezős hitelesítés hello feltétel toorequire a multi-factor authentication teljesül, hello felhasználó le van tiltva.</span><span class="sxs-lookup"><span data-stu-id="bfaac-201">If hello condition toorequire multi-factor authentication is satisfied for a user who is not yet registered for multi-factor authentication, hello user is blocked.</span></span>

<span data-ttu-id="bfaac-202">Ajánlott eljárásként Ha azt szeretné, hogy a multi-factor authentication toorequire a kockázatos bejelentkezések, akkor a következőket:</span><span class="sxs-lookup"><span data-stu-id="bfaac-202">As a best practice, if you want toorequire multi-factor authentication for risky sign-ins, you should:</span></span>

1. <span data-ttu-id="bfaac-203">Hello engedélyezése [a multi-factor authentication regisztráció házirend](#multi-factor-authentication-registration-policy) hello érintett felhasználók.</span><span class="sxs-lookup"><span data-stu-id="bfaac-203">Enable hello [multi-factor authentication registration policy](#multi-factor-authentication-registration-policy) for hello affected users.</span></span>
2. <span data-ttu-id="bfaac-204">Hello érintett felhasználók toologin egy kockázatos munkamenet tooperform a multi-factor Authentication regisztráció megkövetelése</span><span class="sxs-lookup"><span data-stu-id="bfaac-204">Require hello affected users toologin in a non-risky session tooperform a MFA registration</span></span>

<span data-ttu-id="bfaac-205">A lépések végrehajtása biztosítja, hogy a multi-factor authentication a kockázatos bejelentkezéshez szükséges.</span><span class="sxs-lookup"><span data-stu-id="bfaac-205">Completing these steps ensures that multi-factor authentication is required for a risky sign-in.</span></span>

#### <a name="best-practices"></a><span data-ttu-id="bfaac-206">Ajánlott eljárások</span><span class="sxs-lookup"><span data-stu-id="bfaac-206">Best practices</span></span>
<span data-ttu-id="bfaac-207">Válassza a **magas** küszöbérték hello száma szabályzat elindul, és minimálisra csökkenti a hello hatás toousers csökkenti.</span><span class="sxs-lookup"><span data-stu-id="bfaac-207">Choosing a **High** threshold reduces hello number of times a policy is triggered and minimizes hello impact toousers.</span></span>  

<span data-ttu-id="bfaac-208">Azonban nem tartalmazza **alacsony** és **Közepes** bejelentkezések meg van jelölve, a kockázat hello házirend, amely nem blokkolhatják a támadó a sérült biztonságú identitás kihasználhassa.</span><span class="sxs-lookup"><span data-stu-id="bfaac-208">However, it excludes **Low** and **Medium** sign-ins flagged for risk from hello policy, which may not block an attacker from exploiting a compromised identity.</span></span>

<span data-ttu-id="bfaac-209">Ha a beállítás hello házirend,</span><span class="sxs-lookup"><span data-stu-id="bfaac-209">When setting hello policy,</span></span>

* <span data-ttu-id="bfaac-210">Felhasználók, akik nem / nem lehet a többtényezős hitelesítés kihagyása</span><span class="sxs-lookup"><span data-stu-id="bfaac-210">Exclude users who do not/cannot have multi-factor authentication</span></span>
* <span data-ttu-id="bfaac-211">Kizárhat felhasználókat azokon a területeken, ahol hello házirend engedélyezése nincs gyakorlati (például nincs hozzáférési toohelpdesk)</span><span class="sxs-lookup"><span data-stu-id="bfaac-211">Exclude users in locales where enabling hello policy is not practical (for example no access toohelpdesk)</span></span>
* <span data-ttu-id="bfaac-212">Kizárt felhasználók valószínűleg toogenerate nagy mennyiségű hamis-figyelmeztetéséket (fejlesztők, adatbiztonsági elemzők)</span><span class="sxs-lookup"><span data-stu-id="bfaac-212">Exclude users who are likely toogenerate a lot of false-positives (developers, security analysts)</span></span>
* <span data-ttu-id="bfaac-213">Használja a **magas** küszöbérték alatt kezdeti házirend bevezetési, vagy ha a felhasználók számára kihívást kell minimálisra csökkenthető.</span><span class="sxs-lookup"><span data-stu-id="bfaac-213">Use a **High** threshold during initial policy roll out, or if you must minimize challenges seen by end users.</span></span>
* <span data-ttu-id="bfaac-214">Használja a **alacsony** küszöbértéket, ha a szervezet nagyobb biztonságot igényel.</span><span class="sxs-lookup"><span data-stu-id="bfaac-214">Use a **Low**  threshold if your organization requires greater security.</span></span> <span data-ttu-id="bfaac-215">Válassza a **alacsony** küszöbérték vezet be további felhasználói bejelentkezési kihívás, de a biztonság fokozása érdekében.</span><span class="sxs-lookup"><span data-stu-id="bfaac-215">Selecting a **Low** threshold introduces additional user sign-in challenges, but increased security.</span></span>

<span data-ttu-id="bfaac-216">javasolt az alapértelmezett, a legtöbb szervezet tooconfigure szabály hello egy **Közepes** küszöbérték toostrike használhatóság és a biztonság egyensúlyára.</span><span class="sxs-lookup"><span data-stu-id="bfaac-216">hello recommended default for most organizations is tooconfigure a rule for a **Medium** threshold toostrike a balance between usability and security.</span></span>

<span data-ttu-id="bfaac-217">hello bejelentkezési kockázat házirendnek van:</span><span class="sxs-lookup"><span data-stu-id="bfaac-217">hello sign-in risk policy is:</span></span>

* <span data-ttu-id="bfaac-218">Alkalmazott tooall böngésző forgalom és a modern hitelesítést használó bejelentkezéseket.</span><span class="sxs-lookup"><span data-stu-id="bfaac-218">Applied tooall browser traffic and sign-ins using modern authentication.</span></span>
* <span data-ttu-id="bfaac-219">Nem alkalmazott tooapplications helyen összevont hello IDP, például az AD FS hello WS-Trust végpont letiltása a régebbi biztonsági protokollok használatával.</span><span class="sxs-lookup"><span data-stu-id="bfaac-219">Not applied tooapplications using older security protocols by disabling hello WS-Trust endpoint at hello federated IDP, such as ADFS.</span></span>

<span data-ttu-id="bfaac-220">Hello **kockázati események** hello Identity Protection konzolon lap felsorolja az összes eseményt:</span><span class="sxs-lookup"><span data-stu-id="bfaac-220">hello **Risk Events** page in hello Identity Protection console lists all events:</span></span>

* <span data-ttu-id="bfaac-221">Ez a házirend alkalmazott</span><span class="sxs-lookup"><span data-stu-id="bfaac-221">This policy was applied to</span></span>
* <span data-ttu-id="bfaac-222">Tekintse át a hello tevékenység, és megtudhatja, hogy hello művelet megfelelő volt-e vagy sem</span><span class="sxs-lookup"><span data-stu-id="bfaac-222">You can review hello activity and determine whether hello action was appropriate or not</span></span>

<span data-ttu-id="bfaac-223">Hello áttekintését kapcsolódó felhasználói élményt, lásd:</span><span class="sxs-lookup"><span data-stu-id="bfaac-223">For an overview of hello related user experience, see:</span></span>

* [<span data-ttu-id="bfaac-224">Kockázatos bejelentkezési helyreállítási</span><span class="sxs-lookup"><span data-stu-id="bfaac-224">Risky sign-in recovery</span></span>](active-directory-identityprotection-flows.md#risky-sign-in-recovery)
* [<span data-ttu-id="bfaac-225">Kockázatos bejelentkezés letiltva</span><span class="sxs-lookup"><span data-stu-id="bfaac-225">Risky sign-in blocked</span></span>](active-directory-identityprotection-flows.md#risky-sign-in-blocked)  
* [<span data-ttu-id="bfaac-226">Az Azure AD Identity Protection bejelentkezési élmény</span><span class="sxs-lookup"><span data-stu-id="bfaac-226">Sign-in experiences with Azure AD Identity Protection</span></span>](active-directory-identityprotection-flows.md)  

<span data-ttu-id="bfaac-227">**tooopen hello kapcsolódó konfigurációs párbeszédpanel**:</span><span class="sxs-lookup"><span data-stu-id="bfaac-227">**tooopen hello related configuration dialog**:</span></span>

- <span data-ttu-id="bfaac-228">A hello **Azure AD Identity Protection** paneljén, hello **konfigurálása** területén kattintson **bejelentkezési kockázat házirendnek**.</span><span class="sxs-lookup"><span data-stu-id="bfaac-228">On hello **Azure AD Identity Protection** blade, in hello **Configure** section, click **Sign-in risk policy**.</span></span>

    <span data-ttu-id="bfaac-229">![Felhasználói ridk házirend](./media/active-directory-identityprotection/1014.png "felhasználói ridk házirend")</span><span class="sxs-lookup"><span data-stu-id="bfaac-229">![User ridk policy](./media/active-directory-identityprotection/1014.png "User ridk policy")</span></span>



## <a name="users-flagged-for-risk"></a><span data-ttu-id="bfaac-230">Kockázatosként megjelölt felhasználók</span><span class="sxs-lookup"><span data-stu-id="bfaac-230">Users flagged for risk</span></span>

<span data-ttu-id="bfaac-231">Összes aktív [kockázati események](active-directory-identity-protection-risk-events.md) , amely az Azure Active Directory által észlelt, a felhasználó hozzájárul tooa felhasználói kockázat nevű logikai fogalom.</span><span class="sxs-lookup"><span data-stu-id="bfaac-231">All active [risk events](active-directory-identity-protection-risk-events.md) that were detected by Azure Active Directory for a user contribute tooa logical concept called user risk.</span></span> <span data-ttu-id="bfaac-232">A kockázat megjelölt felhasználó egy egy felhasználói fiókot, amely hatékonyságát.</span><span class="sxs-lookup"><span data-stu-id="bfaac-232">A user flagged for risk is an indicator for a user account that might have been compromised.</span></span>

![Kockázatosként megjelölt felhasználók](./media/active-directory-identityprotection/1200.png)


### <a name="user-risk-level"></a><span data-ttu-id="bfaac-234">Felhasználói kockázati szint</span><span class="sxs-lookup"><span data-stu-id="bfaac-234">User risk level</span></span>

<span data-ttu-id="bfaac-235">Egy felhasználó kockázati szint feltüntetése (magas, közepes vagy alacsony) hello valószínűsége, hogy sérült hello felhasználó identitása.</span><span class="sxs-lookup"><span data-stu-id="bfaac-235">A user risk level is an indication (High, Medium, or Low) of hello likelihood that hello user’s identity has been compromised.</span></span> <span data-ttu-id="bfaac-236">Ez alapján van kiszámítva hello felhasználói kockázati események társított a felhasználó identitását.</span><span class="sxs-lookup"><span data-stu-id="bfaac-236">It is calculated based on hello user risk events that are associated with a user's identity.</span></span>

<span data-ttu-id="bfaac-237">hello a kockázati események állapota vagy **aktív** vagy **lezárva**.</span><span class="sxs-lookup"><span data-stu-id="bfaac-237">hello status of a risk event is either **Active** or **Closed**.</span></span> <span data-ttu-id="bfaac-238">Csak a megfelelő események kockázatát **aktív** toohello felhasználói kockázati szint számítási közre.</span><span class="sxs-lookup"><span data-stu-id="bfaac-238">Only risk events that are **Active** contribute toohello user risk level calculation.</span></span>

<span data-ttu-id="bfaac-239">hello felhasználói kockázati szint számítja ki a következő bemenet hello:</span><span class="sxs-lookup"><span data-stu-id="bfaac-239">hello user risk level is calculated using hello following inputs:</span></span>

* <span data-ttu-id="bfaac-240">Hello felhasználói érintő aktív kockázati események</span><span class="sxs-lookup"><span data-stu-id="bfaac-240">Active risk events impacting hello user</span></span>
* <span data-ttu-id="bfaac-241">Ezek az események kockázati szint</span><span class="sxs-lookup"><span data-stu-id="bfaac-241">Risk level of these events</span></span>
* <span data-ttu-id="bfaac-242">E került sor esetleg elvégzett szervizelési műveleteket</span><span class="sxs-lookup"><span data-stu-id="bfaac-242">Whether any remediation actions have been taken</span></span>

<span data-ttu-id="bfaac-243">![Felhasználói kockázatok](./media/active-directory-identityprotection/1031.png "felhasználói kockázatok")</span><span class="sxs-lookup"><span data-stu-id="bfaac-243">![User risks](./media/active-directory-identityprotection/1031.png "User risks")</span></span>

<span data-ttu-id="bfaac-244">Hello felhasználói kockázati szintek toocreate feltételes hozzáférési házirendjeivel, amely megakadályozhatja a kockázatos felhasználók jelentkezik be, vagy őket toosecurely módosíthatják jelszavukat.</span><span class="sxs-lookup"><span data-stu-id="bfaac-244">You can use hello user risk levels toocreate conditional access policies that block risky users from signing in, or force them toosecurely change their password.</span></span>

### <a name="closing-risk-events-manually"></a><span data-ttu-id="bfaac-245">Kockázati események lezárása manuálisan</span><span class="sxs-lookup"><span data-stu-id="bfaac-245">Closing risk events manually</span></span>

<span data-ttu-id="bfaac-246">A legtöbb esetben elvégzett szervizelési műveleteket, mint például egy biztonságos jelszó-átállítási tooautomatically Bezárás kockázati események lépnek meg.</span><span class="sxs-lookup"><span data-stu-id="bfaac-246">In most cases, you will take remediation actions such as a secure password reset tooautomatically close risk events.</span></span> <span data-ttu-id="bfaac-247">Azonban ez nem mindig lehet lehetséges.</span><span class="sxs-lookup"><span data-stu-id="bfaac-247">However, this might not always be possible.</span></span>  
<span data-ttu-id="bfaac-248">Ez helyzet, például hello, ha:</span><span class="sxs-lookup"><span data-stu-id="bfaac-248">This is, for example, hello case, when:</span></span>

* <span data-ttu-id="bfaac-249">A felhasználó Active kockázati események törölve lett</span><span class="sxs-lookup"><span data-stu-id="bfaac-249">A user with Active risk events has been deleted</span></span>
* <span data-ttu-id="bfaac-250">A vizsgálat azt jelzi, hogy a jelentett kockázat esemény hello hiteles felhasználó által lett-e végre.</span><span class="sxs-lookup"><span data-stu-id="bfaac-250">An investigation reveals that a reported risk event has been perform by hello legitimate user</span></span>

<span data-ttu-id="bfaac-251">Mivel a kockázati eseményekről, amelyek **aktív** toohello felhasználói kockázat számítási közre, előfordulhat, hogy a kockázati szintjét alacsonyabb kockázati események manuálisan bezárásával toomanually.</span><span class="sxs-lookup"><span data-stu-id="bfaac-251">Because risk events that are **Active** contribute toohello user risk calculation, you may have toomanually lower a risk level by closing risk events manually.</span></span>  
<span data-ttu-id="bfaac-252">Vizsgálat hello során választhat tootake ezen műveletek toochange hello állapotát a kockázati események bármelyikét:</span><span class="sxs-lookup"><span data-stu-id="bfaac-252">During hello course of investigation, you can choose tootake any of these actions toochange hello status of a risk event:</span></span>

<span data-ttu-id="bfaac-253">![Műveletek](./media/active-directory-identityprotection/34.png "műveletek")</span><span class="sxs-lookup"><span data-stu-id="bfaac-253">![Actions](./media/active-directory-identityprotection/34.png "Actions")</span></span>

* <span data-ttu-id="bfaac-254">**Hárítsa el** – Ha után vizsgálja meg a kockázati események, egy megfelelő szervizelési művelet kívül Identity Protection tartott, és úgy véli, hogy hello kockázat esemény tekintendő bezárul, be van jelölve hello esemény megoldva.</span><span class="sxs-lookup"><span data-stu-id="bfaac-254">**Resolve** - If after investigating a risk event, you took an appropriate remediation action outside Identity Protection, and you believe that hello risk event should be considered closed, mark hello event as Resolved.</span></span> <span data-ttu-id="bfaac-255">Megoldott események hello kockázat esemény állapot tooClosed állítja be, és hello kockázat esemény már nem járul toouser kockázata.</span><span class="sxs-lookup"><span data-stu-id="bfaac-255">Resolved events will set hello risk event’s status tooClosed and hello risk event will no longer contribute toouser risk.</span></span>
* <span data-ttu-id="bfaac-256">**Jelölje meg a vakriasztások** – néhány esetben is vizsgálja meg a kockázati események és helytelenül lett megjelölve, egy kockázatos felderítése.</span><span class="sxs-lookup"><span data-stu-id="bfaac-256">**Mark as false-positive** - In some cases, you may investigate a risk event and discover that it was incorrectly flagged as a risky.</span></span> <span data-ttu-id="bfaac-257">Csökkentheti az ilyen előfordulások száma hello hello kockázat esemény vakriasztások megjelölésével.</span><span class="sxs-lookup"><span data-stu-id="bfaac-257">You can help reduce hello number of such occurrences by marking hello risk event as False-positive.</span></span> <span data-ttu-id="bfaac-258">Ez segít hello gépi tanulási a algoritmusok tooimprove hello besorolás jövőbeli hello hasonló események.</span><span class="sxs-lookup"><span data-stu-id="bfaac-258">This will help hello machine learning algorithms tooimprove hello classification of similar events in hello future.</span></span> <span data-ttu-id="bfaac-259">hello vakriasztások események állapota túl**lezárva** és már nem hozzá toouser kockázata.</span><span class="sxs-lookup"><span data-stu-id="bfaac-259">hello status of false-positive events is too**Closed** and they will no longer contribute toouser risk.</span></span>
* <span data-ttu-id="bfaac-260">**Figyelmen kívül hagyása** – Ha bármilyen szervizelési művelet nem tett, de szeretné hello kockázat esemény toobe eltávolítása hello aktív listájáról, jelölheti meg a kockázati események figyelmen kívül hagyása és hello eseményállapot be lesz zárva.</span><span class="sxs-lookup"><span data-stu-id="bfaac-260">**Ignore** - If you have not taken any remediation action, but want hello risk event toobe removed from hello active list, you can mark a risk event Ignore and hello event status will be Closed.</span></span> <span data-ttu-id="bfaac-261">Figyelmen kívül hagyott események nem járulnak toouser kockázata.</span><span class="sxs-lookup"><span data-stu-id="bfaac-261">Ignored events do not contribute toouser risk.</span></span> <span data-ttu-id="bfaac-262">Ezt a beállítást csak ritka esetekben használható.</span><span class="sxs-lookup"><span data-stu-id="bfaac-262">This option should only be used under unusual circumstances.</span></span>
* <span data-ttu-id="bfaac-263">**Aktiválja újra** -kockázati események manuálisan lezárt (kiválasztásával **megoldásához**, **vakriasztás**, vagy **figyelmen kívül hagyása**) is újra kell aktiválni, hello beállítása eseményállapot biztonsági túl**aktív**.</span><span class="sxs-lookup"><span data-stu-id="bfaac-263">**Reactivate** - Risk events that were manually closed (by choosing **Resolve**, **False positive**, or **Ignore**) can be reactivated, setting hello event status back too**Active**.</span></span> <span data-ttu-id="bfaac-264">Újraaktivált kockázati események toohello felhasználói kockázati szint számítási függ.</span><span class="sxs-lookup"><span data-stu-id="bfaac-264">Reactivated risk events contribute toohello user risk level calculation.</span></span> <span data-ttu-id="bfaac-265">Nem lehet újraaktiválni a kockázati eseményekről zárva a szervizelésen keresztül (például egy biztonságos jelszó-átállítási).</span><span class="sxs-lookup"><span data-stu-id="bfaac-265">Risk events closed through remediation (such as a secure password reset) cannot be reactivated.</span></span>

<span data-ttu-id="bfaac-266">**tooopen hello kapcsolódó konfigurációs párbeszédpanel**:</span><span class="sxs-lookup"><span data-stu-id="bfaac-266">**tooopen hello related configuration dialog**:</span></span>

1. <span data-ttu-id="bfaac-267">A hello **Azure AD Identity Protection** panelen, a **vizsgálat**, kattintson a **kockázati események**.</span><span class="sxs-lookup"><span data-stu-id="bfaac-267">On hello **Azure AD Identity Protection** blade, under **Investigate**, click **Risk events**.</span></span>

    <span data-ttu-id="bfaac-268">![Manuális jelszó-átállítási](./media/active-directory-identityprotection/1002.png "manuális jelszó alaphelyzetbe állítása")</span><span class="sxs-lookup"><span data-stu-id="bfaac-268">![Manual password reset](./media/active-directory-identityprotection/1002.png "Manual password reset")</span></span>
2. <span data-ttu-id="bfaac-269">A hello **kockázati események** listában, kattintson a kockázata.</span><span class="sxs-lookup"><span data-stu-id="bfaac-269">In hello **Risk events** list, click a risk.</span></span>

    <span data-ttu-id="bfaac-270">![Manuális jelszó-átállítási](./media/active-directory-identityprotection/1003.png "manuális jelszó alaphelyzetbe állítása")</span><span class="sxs-lookup"><span data-stu-id="bfaac-270">![Manual password reset](./media/active-directory-identityprotection/1003.png "Manual password reset")</span></span>
3. <span data-ttu-id="bfaac-271">Hello kockázat paneljén kattintson jobb gombbal a felhasználó.</span><span class="sxs-lookup"><span data-stu-id="bfaac-271">On hello risk blade, right-click a user.</span></span>

    <span data-ttu-id="bfaac-272">![Manuális jelszó-átállítási](./media/active-directory-identityprotection/1004.png "manuális jelszó alaphelyzetbe állítása")</span><span class="sxs-lookup"><span data-stu-id="bfaac-272">![Manual password reset](./media/active-directory-identityprotection/1004.png "Manual password reset")</span></span>

### <a name="closing-all-risk-events-for-a-user-manually"></a><span data-ttu-id="bfaac-273">Minden kockázati események a felhasználó manuálisan bezárása</span><span class="sxs-lookup"><span data-stu-id="bfaac-273">Closing all risk events for a user manually</span></span>
<span data-ttu-id="bfaac-274">Helyett manuálisan bezárása külön-külön kockázati események egy felhasználó, Azure Active Directory azonosító adatok védelmét is biztosít a metódus tooclose összes esemény egy felhasználó egy kattintással.</span><span class="sxs-lookup"><span data-stu-id="bfaac-274">Instead of manually closing risk events for a user individually, Azure Active Directory Identity Protection also provides you with a method tooclose all events for a user with one click.</span></span>

<span data-ttu-id="bfaac-275">![Műveletek](./media/active-directory-identityprotection/2222.png "műveletek")</span><span class="sxs-lookup"><span data-stu-id="bfaac-275">![Actions](./media/active-directory-identityprotection/2222.png "Actions")</span></span>

<span data-ttu-id="bfaac-276">Amikor rákattint **hagyja figyelmen kívül az összes esemény**, az összes esemény be van zárva, és hatással hello felhasználó már nem veszélyben.</span><span class="sxs-lookup"><span data-stu-id="bfaac-276">When you click **Dismiss all events**, all events are closed and hello affected user is no longer at risk.</span></span>

### <a name="remediating-user-risk-events"></a><span data-ttu-id="bfaac-277">Felmérésével felhasználói kockázati események</span><span class="sxs-lookup"><span data-stu-id="bfaac-277">Remediating user risk events</span></span>

<span data-ttu-id="bfaac-278">A szervizelés egy művelet toosecure identitás vagy egy eszköz, amely korábban gyanús vagy toobe ismert sérült.</span><span class="sxs-lookup"><span data-stu-id="bfaac-278">A remediation is an action toosecure an identity or a device that was previously suspected or known toobe compromised.</span></span> <span data-ttu-id="bfaac-279">A javítási művelet visszaállítja a hello identitás vagy az eszköz tooa biztonságos állapotát, és oldja fel az előző kockázati események társított hello identitás vagy az eszköz.</span><span class="sxs-lookup"><span data-stu-id="bfaac-279">A remediation action restores hello identity or device tooa safe state, and resolves previous risk events associated with hello identity or device.</span></span>

<span data-ttu-id="bfaac-280">tooremediate felhasználói kockázati eseményekről, a következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="bfaac-280">tooremediate user risk events, you can:</span></span>

* <span data-ttu-id="bfaac-281">Hajtsa végre kézzel egy biztonságos jelszó alaphelyzetbe állítása tooremediate felhasználói kockázati események</span><span class="sxs-lookup"><span data-stu-id="bfaac-281">Perform a secure password reset tooremediate user risk events manually</span></span>
* <span data-ttu-id="bfaac-282">Konfigurálja a felhasználói kockázat biztonsági házirend toomitigate vagy automatikusan kijavítani a felhasználó kockázati események</span><span class="sxs-lookup"><span data-stu-id="bfaac-282">Configure a user risk security policy toomitigate or remediate user risk events automatically</span></span>
* <span data-ttu-id="bfaac-283">Hello fertőzött eszköz visszaállítása</span><span class="sxs-lookup"><span data-stu-id="bfaac-283">Re-image hello infected device</span></span>  

#### <a name="manual-secure-password-reset"></a><span data-ttu-id="bfaac-284">Manuális biztonságos jelszó alaphelyzetbe állítása</span><span class="sxs-lookup"><span data-stu-id="bfaac-284">Manual secure password reset</span></span>
<span data-ttu-id="bfaac-285">A biztonságos jelszó alaphelyzetbe állítása egy hatékony szervizelése sok kockázati eseményekről, és végrehajtásakor, automatikusan a kockázati eseményekről bezárja, és újraszámítja hello felhasználói kockázati szintjét.</span><span class="sxs-lookup"><span data-stu-id="bfaac-285">A secure password reset is an effective remediation for many risk events, and when performed, automatically closes these risk events and recalculates hello user risk level.</span></span> <span data-ttu-id="bfaac-286">Hello Identity Protection-irányítópult tooinitiate egy kockázatos felhasználó új jelszót is használhatja.</span><span class="sxs-lookup"><span data-stu-id="bfaac-286">You can use hello Identity Protection dashboard tooinitiate a password reset for a risky user.</span></span>

<span data-ttu-id="bfaac-287">hello kapcsolódó párbeszédpanel biztosít két különböző módszereket tooreset jelszót:</span><span class="sxs-lookup"><span data-stu-id="bfaac-287">hello related dialog provides two different methods tooreset a password:</span></span>

<span data-ttu-id="bfaac-288">**Jelszó-átállítási** – Itt adhatja meg **hello felhasználói tooreset a jelszó szükséges** tooallow hello felhasználói tooself-helyreállítás, ha hello a felhasználó a multi-factor authentication van regisztrálva.</span><span class="sxs-lookup"><span data-stu-id="bfaac-288">**Reset password** - Select **Require hello user tooreset their password** tooallow hello user tooself-recover if hello user has registered for multi-factor authentication.</span></span> <span data-ttu-id="bfaac-289">Hello felhasználói tovább bejelentkezés során a hello felhasználó multi-factor Authentication hitelesítést sikeresen ellenőrző kérdés szükséges toosolve és majd, a kényszerített toochange hello jelszó lesz.</span><span class="sxs-lookup"><span data-stu-id="bfaac-289">During hello user's next sign-in, hello user will be required toosolve a multi-factor authentication challenge successfully and then, forced toochange hello password.</span></span> <span data-ttu-id="bfaac-290">Ez a lehetőség nem érhető el, ha hello felhasználói fiók már nem regisztrált multi-factor Authentication hitelesítést.</span><span class="sxs-lookup"><span data-stu-id="bfaac-290">This option isn't available if hello user account is not already registered multi-factor authentication.</span></span>

<span data-ttu-id="bfaac-291">**Ideiglenes jelszó** – Itt adhatja meg **létrehozni egy ideiglenes jelszót** tooimmediately érvénytelenné válnak a meglévő hello jelszó, és hozzon létre egy hello felhasználó új ideiglenes jelszót.</span><span class="sxs-lookup"><span data-stu-id="bfaac-291">**Temporary password** - Select **Generate a temporary password** tooimmediately invalidate hello existing password, and create a new temporary password for hello user.</span></span> <span data-ttu-id="bfaac-292">Hello új ideiglenes jelszó tooan másodlagos e-mail címére hello felhasználó vagy felhasználói toohello manager küldése.</span><span class="sxs-lookup"><span data-stu-id="bfaac-292">Send hello new temporary password tooan alternate email address for hello user or toohello user's manager.</span></span> <span data-ttu-id="bfaac-293">Mivel hello jelszó ideiglenes, hello felhasználó fog felszólító toochange hello jelszó bejelentkezés után.</span><span class="sxs-lookup"><span data-stu-id="bfaac-293">Because hello password is temporary, hello user will be prompted toochange hello password upon sign-in.</span></span>

<span data-ttu-id="bfaac-294">![Házirend](./media/active-directory-identityprotection/1005.png "házirend")</span><span class="sxs-lookup"><span data-stu-id="bfaac-294">![Policy](./media/active-directory-identityprotection/1005.png "Policy")</span></span>

<span data-ttu-id="bfaac-295">**tooopen hello kapcsolódó konfigurációs párbeszédpanel**:</span><span class="sxs-lookup"><span data-stu-id="bfaac-295">**tooopen hello related configuration dialog**:</span></span>

1. <span data-ttu-id="bfaac-296">A hello **Azure AD Identity Protection** panelen kattintson a **felhasználók meg van jelölve, a kockázat**.</span><span class="sxs-lookup"><span data-stu-id="bfaac-296">On hello **Azure AD Identity Protection** blade, click **Users flagged for risk**.</span></span>

    <span data-ttu-id="bfaac-297">![Manuális jelszó-átállítási](./media/active-directory-identityprotection/1006.png "manuális jelszó alaphelyzetbe állítása")</span><span class="sxs-lookup"><span data-stu-id="bfaac-297">![Manual password reset](./media/active-directory-identityprotection/1006.png "Manual password reset")</span></span>
2. <span data-ttu-id="bfaac-298">A felhasználók hello listáról válassza ki a felhasználó legalább egy kockázati események.</span><span class="sxs-lookup"><span data-stu-id="bfaac-298">From hello list of users, select a user with at least one risk events.</span></span>

    <span data-ttu-id="bfaac-299">![Manuális jelszó-átállítási](./media/active-directory-identityprotection/1007.png "manuális jelszó alaphelyzetbe állítása")</span><span class="sxs-lookup"><span data-stu-id="bfaac-299">![Manual password reset](./media/active-directory-identityprotection/1007.png "Manual password reset")</span></span>
3. <span data-ttu-id="bfaac-300">Hello felhasználói paneljén kattintson **jelszó-átállítási**.</span><span class="sxs-lookup"><span data-stu-id="bfaac-300">On hello user blade, click **Reset password**.</span></span>

    <span data-ttu-id="bfaac-301">![Manuális jelszó-átállítási](./media/active-directory-identityprotection/1008.png "manuális jelszó alaphelyzetbe állítása")</span><span class="sxs-lookup"><span data-stu-id="bfaac-301">![Manual password reset](./media/active-directory-identityprotection/1008.png "Manual password reset")</span></span>

### <a name="user-risk-security-policy"></a><span data-ttu-id="bfaac-302">Felhasználói kockázat biztonsági házirend</span><span class="sxs-lookup"><span data-stu-id="bfaac-302">User risk security policy</span></span>
<span data-ttu-id="bfaac-303">A felhasználó kockázati biztonsági házirend egy feltételes hozzáférési szabályzatot, amely kiértékeli a hello kockázati szint tooa adott felhasználó és előre meghatározott feltételt és a szabályok alapján a szervizelés és a megoldás műveleteket hajt végre.</span><span class="sxs-lookup"><span data-stu-id="bfaac-303">A user risk security policy is a conditional access policy that evaluates hello risk level tooa specific user and applies remediation and mitigation actions based on predefined conditions and rules.</span></span>

<span data-ttu-id="bfaac-304">![Felhasználói ridk házirend](./media/active-directory-identityprotection/1009.png "felhasználói ridk házirend")</span><span class="sxs-lookup"><span data-stu-id="bfaac-304">![User ridk policy](./media/active-directory-identityprotection/1009.png "User ridk policy")</span></span>

<span data-ttu-id="bfaac-305">Az Azure AD Identity Protection segítségével felügyelheti a hello megoldás, a javítási azáltal, hogy kockázat megjelölt felhasználók:</span><span class="sxs-lookup"><span data-stu-id="bfaac-305">Azure AD Identity Protection helps you manage hello mitigation and remediation of users flagged for risk by enabling you to:</span></span>

* <span data-ttu-id="bfaac-306">Állítsa be a felhasználók és csoportok hello házirend vonatkozik hello:</span><span class="sxs-lookup"><span data-stu-id="bfaac-306">Set hello users and groups hello policy applies to:</span></span>

    <span data-ttu-id="bfaac-307">![Felhasználói ridk házirend](./media/active-directory-identityprotection/1010.png "felhasználói ridk házirend")</span><span class="sxs-lookup"><span data-stu-id="bfaac-307">![User ridk policy](./media/active-directory-identityprotection/1010.png "User ridk policy")</span></span>
* <span data-ttu-id="bfaac-308">Hello felhasználói kockázati szint küszöbértéke (alacsony, közepes vagy magas), amely elindítja a hello házirend beállítása:</span><span class="sxs-lookup"><span data-stu-id="bfaac-308">Set hello user risk level threshold (low, medium, or high) that triggers hello policy:</span></span>

    <span data-ttu-id="bfaac-309">![Felhasználói ridk házirend](./media/active-directory-identityprotection/1011.png "felhasználói ridk házirend")</span><span class="sxs-lookup"><span data-stu-id="bfaac-309">![User ridk policy](./media/active-directory-identityprotection/1011.png "User ridk policy")</span></span>
* <span data-ttu-id="bfaac-310">Set hello vezérlők toobe lépnek érvénybe, ha elindítja a hello házirend:</span><span class="sxs-lookup"><span data-stu-id="bfaac-310">Set hello controls toobe enforced when hello policy triggers:</span></span>

    <span data-ttu-id="bfaac-311">![Felhasználói ridk házirend](./media/active-directory-identityprotection/1012.png "felhasználói ridk házirend")</span><span class="sxs-lookup"><span data-stu-id="bfaac-311">![User ridk policy](./media/active-directory-identityprotection/1012.png "User ridk policy")</span></span>
* <span data-ttu-id="bfaac-312">A házirend kapcsoló hello állapotát:</span><span class="sxs-lookup"><span data-stu-id="bfaac-312">Switch hello state of your policy:</span></span>

    <span data-ttu-id="bfaac-313">![Felhasználói ridk házirend](./media/active-directory-identityprotection/403.png "MFA-regisztráció")</span><span class="sxs-lookup"><span data-stu-id="bfaac-313">![User ridk policy](./media/active-directory-identityprotection/403.png "MFA Registration")</span></span>
* <span data-ttu-id="bfaac-314">Ellenőrizze, és értékelje hello hatással bír a változtatás aktiválás előtt:</span><span class="sxs-lookup"><span data-stu-id="bfaac-314">Review and evaluate hello impact of a change before activating it:</span></span>

    <span data-ttu-id="bfaac-315">![Felhasználói ridk házirend](./media/active-directory-identityprotection/1013.png "felhasználói ridk házirend")</span><span class="sxs-lookup"><span data-stu-id="bfaac-315">![User ridk policy](./media/active-directory-identityprotection/1013.png "User ridk policy")</span></span>

<span data-ttu-id="bfaac-316">Válassza a **magas** küszöbérték hello száma szabályzat elindul, és minimálisra csökkenti a hello hatás toousers csökkenti.</span><span class="sxs-lookup"><span data-stu-id="bfaac-316">Choosing a **High** threshold reduces hello number of times a policy is triggered and minimizes hello impact toousers.</span></span>
<span data-ttu-id="bfaac-317">Azonban nem tartalmazza **alacsony** és **Közepes** hello házirendet, amely előfordulhat, hogy nem biztonságos identitások vagy eszközöket, amelyek fenyegeti megjelölt felhasználók korábban gyanús vagy sérült toobe ismert.</span><span class="sxs-lookup"><span data-stu-id="bfaac-317">However, it excludes **Low** and **Medium** users flagged for risk from hello policy, which may not secure identities or devices that were previously suspected or known toobe compromised.</span></span>

<span data-ttu-id="bfaac-318">Ha a beállítás hello házirend,</span><span class="sxs-lookup"><span data-stu-id="bfaac-318">When setting hello policy,</span></span>

* <span data-ttu-id="bfaac-319">Kizárt felhasználók valószínűleg toogenerate nagy mennyiségű hamis-figyelmeztetéséket (fejlesztők, adatbiztonsági elemzők)</span><span class="sxs-lookup"><span data-stu-id="bfaac-319">Exclude users who are likely toogenerate a lot of false-positives (developers, security analysts)</span></span>
* <span data-ttu-id="bfaac-320">Kizárhat felhasználókat azokon a területeken, ahol hello házirend engedélyezése nincs gyakorlati (például nincs hozzáférési toohelpdesk)</span><span class="sxs-lookup"><span data-stu-id="bfaac-320">Exclude users in locales where enabling hello policy is not practical (for example no access toohelpdesk)</span></span>
* <span data-ttu-id="bfaac-321">Használja a **magas** küszöbérték alatt kezdeti házirend bevezetési, vagy ha a felhasználók számára kihívást kell minimálisra csökkenthető.</span><span class="sxs-lookup"><span data-stu-id="bfaac-321">Use a **High** threshold during initial policy roll out, or if you must minimize challenges seen by end users.</span></span>
* <span data-ttu-id="bfaac-322">Használja a **alacsony** küszöbértéket, ha a szervezet nagyobb biztonságot igényel.</span><span class="sxs-lookup"><span data-stu-id="bfaac-322">Use a **Low** threshold if your organization requires greater security.</span></span> <span data-ttu-id="bfaac-323">Válassza a **alacsony** küszöbérték vezet be további felhasználói bejelentkezési kihívás, de a biztonság fokozása érdekében.</span><span class="sxs-lookup"><span data-stu-id="bfaac-323">Selecting a **Low** threshold introduces additional user sign-in challenges, but increased security.</span></span>

<span data-ttu-id="bfaac-324">javasolt az alapértelmezett, a legtöbb szervezet tooconfigure szabály hello egy **Közepes** küszöbérték toostrike használhatóság és a biztonság egyensúlyára.</span><span class="sxs-lookup"><span data-stu-id="bfaac-324">hello recommended default for most organizations is tooconfigure a rule for a **Medium** threshold toostrike a balance between usability and security.</span></span>

<span data-ttu-id="bfaac-325">Hello áttekintését kapcsolódó felhasználói élményt, lásd:</span><span class="sxs-lookup"><span data-stu-id="bfaac-325">For an overview of hello related user experience, see:</span></span>

* <span data-ttu-id="bfaac-326">[Sérült a fiók a helyreállítási folyamat](active-directory-identityprotection-flows.md#compromised-account-recovery).</span><span class="sxs-lookup"><span data-stu-id="bfaac-326">[Compromised account recovery flow](active-directory-identityprotection-flows.md#compromised-account-recovery).</span></span>  
* <span data-ttu-id="bfaac-327">[Sérült a fiók blokkolva van folyamata](active-directory-identityprotection-flows.md#compromised-account-blocked).</span><span class="sxs-lookup"><span data-stu-id="bfaac-327">[Compromised account blocked flow](active-directory-identityprotection-flows.md#compromised-account-blocked).</span></span>  

<span data-ttu-id="bfaac-328">**tooopen hello kapcsolódó konfigurációs párbeszédpanel**:</span><span class="sxs-lookup"><span data-stu-id="bfaac-328">**tooopen hello related configuration dialog**:</span></span>

- <span data-ttu-id="bfaac-329">A hello **Azure AD Identity Protection** paneljén, hello **konfigurálása** területén kattintson **felhasználói kockázat házirendnek**.</span><span class="sxs-lookup"><span data-stu-id="bfaac-329">On hello **Azure AD Identity Protection** blade, in hello **Configure** section, click **User risk policy**.</span></span>

    <span data-ttu-id="bfaac-330">![Felhasználói ridk házirend](./media/active-directory-identityprotection/1009.png "felhasználói ridk házirend")</span><span class="sxs-lookup"><span data-stu-id="bfaac-330">![User ridk policy](./media/active-directory-identityprotection/1009.png "User ridk policy")</span></span>

### <a name="mitigating-user-risk-events"></a><span data-ttu-id="bfaac-331">Felhasználói kockázati események orvoslása</span><span class="sxs-lookup"><span data-stu-id="bfaac-331">Mitigating user risk events</span></span>
<span data-ttu-id="bfaac-332">A rendszergazdák állíthatja be egy felhasználó kockázat biztonsági házirend tooblock felhasználók bejelentkezés után attól függően, hogy hello kockázati szintjét.</span><span class="sxs-lookup"><span data-stu-id="bfaac-332">Administrators can set a user risk security policy tooblock users upon sign-in depending on hello risk level.</span></span>

<span data-ttu-id="bfaac-333">Blokkolja a bejelentkezés:</span><span class="sxs-lookup"><span data-stu-id="bfaac-333">Blocking a sign-in:</span></span>

* <span data-ttu-id="bfaac-334">Megakadályozza, hogy a hello hello érintett felhasználó új felhasználói kockázati események előállítását</span><span class="sxs-lookup"><span data-stu-id="bfaac-334">Prevents hello generation of new user risk events for hello affected user</span></span>
* <span data-ttu-id="bfaac-335">Lehetővé teszi, hogy a rendszergazdák toomanually hello felhasználói identitás érintő hello kockázati eseményekről megoldásáról, majd visszaállítja azt tooa biztonsági állapota</span><span class="sxs-lookup"><span data-stu-id="bfaac-335">Enables administrators toomanually remediate hello risk events affecting hello user's identity and restore it tooa secure state</span></span>



## <a name="multi-factor-authentication-registration-policy"></a><span data-ttu-id="bfaac-336">A multi-factor authentication regisztráció házirend</span><span class="sxs-lookup"><span data-stu-id="bfaac-336">Multi-factor authentication registration policy</span></span>
<span data-ttu-id="bfaac-337">Az Azure többtényezős hitelesítés a ellenőrzésére, akik csupán felhasználónévvel és jelszóval hello használatát igénylő módszer.</span><span class="sxs-lookup"><span data-stu-id="bfaac-337">Azure multi-factor authentication is a method of verifying who you are that requires hello use of more than just a username and password.</span></span> <span data-ttu-id="bfaac-338">Biztonsági toouser bejelentkezéseket és tranzakciókat második réteget biztosít.</span><span class="sxs-lookup"><span data-stu-id="bfaac-338">It provides a second layer of security toouser sign-ins and transactions.</span></span>  
<span data-ttu-id="bfaac-339">Azt javasoljuk, hogy az Azure többtényezős hitelesítés a felhasználói bejelentkezések szükséges el, mert azt:</span><span class="sxs-lookup"><span data-stu-id="bfaac-339">We recommend that you require Azure multi-factor authentication for user sign-ins because it:</span></span>

* <span data-ttu-id="bfaac-340">Erős hitelesítés, könnyen ellenőrzési lehetőségek széles kézbesíti</span><span class="sxs-lookup"><span data-stu-id="bfaac-340">Delivers strong authentication with a range of easy verification options</span></span>
* <span data-ttu-id="bfaac-341">A szervezet tooprotect előkészítésében kulcsfontosságú szerepet játszik, és a fiók biztonság sérüléseinek helyreállítása</span><span class="sxs-lookup"><span data-stu-id="bfaac-341">Plays a key role in preparing your organization tooprotect and recover from account compromises</span></span>

<span data-ttu-id="bfaac-342">![Felhasználói ridk házirend](./media/active-directory-identityprotection/1019.png "felhasználói ridk házirend")</span><span class="sxs-lookup"><span data-stu-id="bfaac-342">![User ridk policy](./media/active-directory-identityprotection/1019.png "User ridk policy")</span></span>

<span data-ttu-id="bfaac-343">További részletekért lásd: [Mi az Azure multi-factor Authentication?](../multi-factor-authentication/multi-factor-authentication.md)</span><span class="sxs-lookup"><span data-stu-id="bfaac-343">For more details, see [What is Azure Multi-Factor Authentication?](../multi-factor-authentication/multi-factor-authentication.md)</span></span>

<span data-ttu-id="bfaac-344">Az Azure AD Identity Protection segítségével felügyelheti a hello kiépítése a multi-factor authentication regisztráció úgy konfigurálja a házirendet, amely lehetővé teszi:</span><span class="sxs-lookup"><span data-stu-id="bfaac-344">Azure AD Identity Protection helps you manage hello roll-out of multi-factor authentication registration by configuring a policy that enables you to:</span></span>

* <span data-ttu-id="bfaac-345">Állítsa be a felhasználók és csoportok hello házirend vonatkozik hello:</span><span class="sxs-lookup"><span data-stu-id="bfaac-345">Set hello users and groups hello policy applies to:</span></span>

    <span data-ttu-id="bfaac-346">![Többtényezős hitelesítési szabályzat](./media/active-directory-identityprotection/1020.png "többtényezős hitelesítési szabályzat")</span><span class="sxs-lookup"><span data-stu-id="bfaac-346">![MFA policy](./media/active-directory-identityprotection/1020.png "MFA policy")</span></span>
* <span data-ttu-id="bfaac-347">Set hello vezérlők toobe lépnek érvénybe, ha elindítja a hello házirend::</span><span class="sxs-lookup"><span data-stu-id="bfaac-347">Set hello controls toobe enforced when hello policy triggers::</span></span>  

    <span data-ttu-id="bfaac-348">![Többtényezős hitelesítési szabályzat](./media/active-directory-identityprotection/1021.png "többtényezős hitelesítési szabályzat")</span><span class="sxs-lookup"><span data-stu-id="bfaac-348">![MFA policy](./media/active-directory-identityprotection/1021.png "MFA policy")</span></span>
* <span data-ttu-id="bfaac-349">A házirend kapcsoló hello állapotát:</span><span class="sxs-lookup"><span data-stu-id="bfaac-349">Switch hello state of your policy:</span></span>

    <span data-ttu-id="bfaac-350">![Többtényezős hitelesítési szabályzat](./media/active-directory-identityprotection/403.png "többtényezős hitelesítési szabályzat")</span><span class="sxs-lookup"><span data-stu-id="bfaac-350">![MFA policy](./media/active-directory-identityprotection/403.png "MFA policy")</span></span>
* <span data-ttu-id="bfaac-351">Hello aktuális regisztrációs állapotának megtekintése:</span><span class="sxs-lookup"><span data-stu-id="bfaac-351">View hello current registration status:</span></span>

    <span data-ttu-id="bfaac-352">![Többtényezős hitelesítési szabályzat](./media/active-directory-identityprotection/1022.png "többtényezős hitelesítési szabályzat")</span><span class="sxs-lookup"><span data-stu-id="bfaac-352">![MFA policy](./media/active-directory-identityprotection/1022.png "MFA policy")</span></span>

<span data-ttu-id="bfaac-353">Hello áttekintését kapcsolódó felhasználói élményt, lásd:</span><span class="sxs-lookup"><span data-stu-id="bfaac-353">For an overview of hello related user experience, see:</span></span>

* <span data-ttu-id="bfaac-354">[A multi-factor authentication regisztrációs folyamat](active-directory-identityprotection-flows.md#multi-factor-authentication-registration).</span><span class="sxs-lookup"><span data-stu-id="bfaac-354">[Multi-factor authentication registration flow](active-directory-identityprotection-flows.md#multi-factor-authentication-registration).</span></span>  
* <span data-ttu-id="bfaac-355">[Bejelentkezés során lép az Azure AD Identity Protection](active-directory-identityprotection-flows.md).</span><span class="sxs-lookup"><span data-stu-id="bfaac-355">[Sign-in experiences with Azure AD Identity Protection](active-directory-identityprotection-flows.md).</span></span>  

<span data-ttu-id="bfaac-356">**tooopen hello kapcsolódó konfigurációs párbeszédpanel**:</span><span class="sxs-lookup"><span data-stu-id="bfaac-356">**tooopen hello related configuration dialog**:</span></span>

- <span data-ttu-id="bfaac-357">A hello **Azure AD Identity Protection** paneljén, hello **konfigurálása** kattintson **a multi-factor authentication regisztráció**.</span><span class="sxs-lookup"><span data-stu-id="bfaac-357">On hello **Azure AD Identity Protection** blade, in hello **Configure** section, click **Multi-factor authentication registration**.</span></span>

    <span data-ttu-id="bfaac-358">![Többtényezős hitelesítési szabályzat](./media/active-directory-identityprotection/1019.png "többtényezős hitelesítési szabályzat")</span><span class="sxs-lookup"><span data-stu-id="bfaac-358">![MFA policy](./media/active-directory-identityprotection/1019.png "MFA policy")</span></span>

## <a name="next-steps"></a><span data-ttu-id="bfaac-359">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="bfaac-359">Next steps</span></span>
* [<span data-ttu-id="bfaac-360">9. csatornán: Az Azure AD és az Identity: Identity Protection előzetes kiadásának</span><span class="sxs-lookup"><span data-stu-id="bfaac-360">Channel 9: Azure AD and Identity Show: Identity Protection Preview</span></span>](https://channel9.msdn.com/Series/Azure-AD-Identity/Azure-AD-and-Identity-Show-Identity-Protection-Preview)

* [<span data-ttu-id="bfaac-361">Az Azure Active Directory-identitás védelem engedélyezése</span><span class="sxs-lookup"><span data-stu-id="bfaac-361">Enabling Azure Active Directory Identity Protection</span></span>](active-directory-identityprotection-enable.md)

* [<span data-ttu-id="bfaac-362">Azure Active Directory Identity Protection által észlelt biztonsági rések</span><span class="sxs-lookup"><span data-stu-id="bfaac-362">Vulnerabilities detected by Azure Active Directory Identity Protection</span></span>](active-directory-identityprotection-vulnerabilities.md)

* [<span data-ttu-id="bfaac-363">Az Azure Active Directory kockázati események</span><span class="sxs-lookup"><span data-stu-id="bfaac-363">Azure Active Directory risk events</span></span>](active-directory-identity-protection-risk-events.md)

* [<span data-ttu-id="bfaac-364">Az Azure Active Directory Identity Protection-értesítések</span><span class="sxs-lookup"><span data-stu-id="bfaac-364">Azure Active Directory Identity Protection notifications</span></span>](active-directory-identityprotection-notifications.md)

* [<span data-ttu-id="bfaac-365">Az Azure Active Directory Identity Protection-forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="bfaac-365">Azure Active Directory Identity Protection playbook</span></span>](active-directory-identityprotection-playbook.md)

* [<span data-ttu-id="bfaac-366">Az Azure Active Directory-Identity Protection-szószedet</span><span class="sxs-lookup"><span data-stu-id="bfaac-366">Azure Active Directory Identity Protection glossary</span></span>](active-directory-identityprotection-glossary.md)

* [<span data-ttu-id="bfaac-367">Az Azure AD Identity Protection bejelentkezési élmény</span><span class="sxs-lookup"><span data-stu-id="bfaac-367">Sign-in experiences with Azure AD Identity Protection</span></span>](active-directory-identityprotection-flows.md)

* [<span data-ttu-id="bfaac-368">Az Azure Active Directory Identity Protection - hogyan toounblock felhasználók</span><span class="sxs-lookup"><span data-stu-id="bfaac-368">Azure Active Directory Identity Protection - How toounblock users</span></span>](active-directory-identityprotection-unblock-howto.md)

* [<span data-ttu-id="bfaac-369">Ismerkedés az Azure Active Directory azonosító adatok védelmét és a Microsoft Graph</span><span class="sxs-lookup"><span data-stu-id="bfaac-369">Get started with Azure Active Directory Identity Protection and Microsoft Graph</span></span>](active-directory-identityprotection-graph-getting-started.md)
