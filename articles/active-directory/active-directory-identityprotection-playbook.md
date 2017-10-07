---
title: "Active Directory Identity Protection-forgatókönyv aaaAzure |} Microsoft Docs"
description: "Ismerje meg, az Azure AD Identity Protection miként toolimit hello azon képessége, egy támadó tooexploit egy sérült biztonságú identitás vagy az eszköz és a toosecure identitás vagy egy eszköz, amely korábban gyanús vagy ismert toobe sérült."
services: active-directory
keywords: "az Azure active directory azonosító adatok védelmét, a cloud app discovery, alkalmazások, biztonság, kockázat, kockázati szint, biztonsági rés, biztonsági házirend kezelése"
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 60836abf-f0e9-459d-b344-8e06b8341d25
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 6252bc133fc0c0f84800ee245a04bbf62d4cd25b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-identity-protection-playbook"></a><span data-ttu-id="b098f-104">Az Azure Active Directory Identity Protection-forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="b098f-104">Azure Active Directory Identity Protection playbook</span></span>
<span data-ttu-id="b098f-105">Ez a forgatókönyv segítséget:</span><span class="sxs-lookup"><span data-stu-id="b098f-105">This playbook helps you to:</span></span>

* <span data-ttu-id="b098f-106">Amelyek kockázati eseményekről és biztonsági rések hello Identity Protection környezetben adatok feltöltése</span><span class="sxs-lookup"><span data-stu-id="b098f-106">Populate data in hello Identity Protection environment by simulating risk events and vulnerabilities</span></span>
* <span data-ttu-id="b098f-107">Kockázati-alapú feltételes hozzáférés szabályzatainak beállítása, és ezek a házirendek hello hatásának tesztelése</span><span class="sxs-lookup"><span data-stu-id="b098f-107">Set up risk-based conditional access policies and test hello impact of these policies</span></span>

## <a name="simulating-risk-events"></a><span data-ttu-id="b098f-108">Kockázati események szimulálása</span><span class="sxs-lookup"><span data-stu-id="b098f-108">Simulating Risk Events</span></span>
<span data-ttu-id="b098f-109">Ez a szakasz biztosít lépéseket a következő kockázatok eseménytípusok hello szimulálva:</span><span class="sxs-lookup"><span data-stu-id="b098f-109">This section provides you with steps for simulating hello following risk event types:</span></span>

* <span data-ttu-id="b098f-110">Bejelentkezések névtelen IP-címről (egyszerű)</span><span class="sxs-lookup"><span data-stu-id="b098f-110">Sign-ins from anonymous IP addresses (easy)</span></span>
* <span data-ttu-id="b098f-111">Bejelentkezések ismeretlen helyekről (közepes)</span><span class="sxs-lookup"><span data-stu-id="b098f-111">Sign-ins from unfamiliar locations (moderate)</span></span>
* <span data-ttu-id="b098f-112">Lehetetlen odautazás tooatypical helyek (nehéz)</span><span class="sxs-lookup"><span data-stu-id="b098f-112">Impossible travel tooatypical locations (difficult)</span></span>

<span data-ttu-id="b098f-113">Más kockázati események nem szimulált, biztonságos módon.</span><span class="sxs-lookup"><span data-stu-id="b098f-113">Other risk events cannot be simulated in a secure manner.</span></span>

### <a name="sign-ins-from-anonymous-ip-addresses"></a><span data-ttu-id="b098f-114">Névtelen IP-címről történő bejelentkezések</span><span class="sxs-lookup"><span data-stu-id="b098f-114">Sign-ins from anonymous IP addresses</span></span>
<span data-ttu-id="b098f-115">A kockázat esemény a névtelen proxy IP-címként azonosított IP-cím sikeresen bejelentkezett felhasználók azonosítja.</span><span class="sxs-lookup"><span data-stu-id="b098f-115">This risk event type identifies users who have successfully signed in from an IP address that has been identified as an anonymous proxy IP address.</span></span> <span data-ttu-id="b098f-116">Ezek proxyk olyan személyek számára, akik toohide az eszköz IP-címet használja, és a rosszindulatú behatolással szemben használható.</span><span class="sxs-lookup"><span data-stu-id="b098f-116">These proxies are used by people who want toohide their device’s IP address and may be used for malicious intent.</span></span>

<span data-ttu-id="b098f-117">**névtelen IP, a bejelentkezés toosimulate hajtsa végre a lépéseket követve hello**:</span><span class="sxs-lookup"><span data-stu-id="b098f-117">**toosimulate a sign-in from an anonymous IP, perform hello following steps**:</span></span>

1. <span data-ttu-id="b098f-118">Töltse le a hello [Tor böngésző](https://www.torproject.org/projects/torbrowser.html.en).</span><span class="sxs-lookup"><span data-stu-id="b098f-118">Download hello [Tor Browser](https://www.torproject.org/projects/torbrowser.html.en).</span></span>
2. <span data-ttu-id="b098f-119">Hello Tor böngészőben nyissa meg a túl[https://myapps.microsoft.com](https://myapps.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="b098f-119">Using hello Tor Browser, navigate too[https://myapps.microsoft.com](https://myapps.microsoft.com).</span></span>   
3. <span data-ttu-id="b098f-120">Azt szeretné, hogy a hello tooappear hello fiók hitelesítő adatainak megadásával hello **névtelen IP-címekről bejelentkezések** jelentés.</span><span class="sxs-lookup"><span data-stu-id="b098f-120">Enter hello credentials of hello account you want tooappear in hello **Sign-ins from anonymous IP addresses** report.</span></span>

<span data-ttu-id="b098f-121">hello bejelentkezés fog megjelenni a hello Identity Protection-irányítópulton 5 percen belül.</span><span class="sxs-lookup"><span data-stu-id="b098f-121">hello sign-in will show up on hello Identity Protection dashboard within 5 minutes.</span></span> 

### <a name="sign-ins-from-unfamiliar-locations"></a><span data-ttu-id="b098f-122">Ismeretlen helyekről történt bejelentkezések</span><span class="sxs-lookup"><span data-stu-id="b098f-122">Sign-ins from unfamiliar locations</span></span>
<span data-ttu-id="b098f-123">hello ismeretlen helyek kockázat egy egy valós idejű bejelentkezési értékelési mechanizmus, amely korábbi bejelentkezési helyek figyelembe veszi (szélesség, IP / hosszúság és a ASN) toodetermine új / ismeretlen helyét.</span><span class="sxs-lookup"><span data-stu-id="b098f-123">hello unfamiliar locations risk is a real-time sign-in evaluation mechanism that considers past sign-in locations (IP, Latitude / Longitude and ASN) toodetermine new / unfamiliar locations.</span></span> <span data-ttu-id="b098f-124">hello rendszer tárolja a korábbi IP-címek, szélesség / hosszúság, és a ASN-eket, hogy egy felhasználó és ismerős toobe helyeken.</span><span class="sxs-lookup"><span data-stu-id="b098f-124">hello system stores previous IPs, Latitude / Longitude, and ASNs of a user and considers these toobe familiar locations.</span></span> <span data-ttu-id="b098f-125">A bejelentkezési helye akkor tekinthető ismeri, ha hello bejelentkezési helye nem egyezik hello meglévő ismerős helyeken.</span><span class="sxs-lookup"><span data-stu-id="b098f-125">A sign-in location is considered unfamiliar if hello sign-in location does not match any of hello existing familiar locations.</span></span>

<span data-ttu-id="b098f-126">Az Azure Active Directory azonosító adatok védelmét:</span><span class="sxs-lookup"><span data-stu-id="b098f-126">Azure Active Directory Identity Protection:</span></span>  

* <span data-ttu-id="b098f-127">kezdeti tanulási időszaka 14 nap során, ami azt nem ez a jelző bármely új helyek ismeretlen helyként.</span><span class="sxs-lookup"><span data-stu-id="b098f-127">has an initial learning period of 14 days during which it does not flag any new locations as unfamiliar locations.</span></span>
* <span data-ttu-id="b098f-128">figyelmen kívül hagyja az olyan ismerős eszközökkel és földrajzilag Bezárás tooan meglévő ismerős hely helyeken bejelentkezéseket.</span><span class="sxs-lookup"><span data-stu-id="b098f-128">ignores sign-ins from familiar devices and locations that are geographically close tooan existing familiar location.</span></span>

<span data-ttu-id="b098f-129">Ismeretlen helyek toosimulate, vannak toosign egy helyről, és az eszköz, amely hello fiók még nem jelentkezett be az előtt.</span><span class="sxs-lookup"><span data-stu-id="b098f-129">toosimulate unfamiliar locations, you have toosign in from a location and device that hello account has not signed in from before.</span></span> 

<span data-ttu-id="b098f-130">**a bejelentkezés egy ismeretlen helyről toosimulate hajtsa végre a lépéseket követve hello**:</span><span class="sxs-lookup"><span data-stu-id="b098f-130">**toosimulate a sign-in from an unfamiliar location, perform hello following steps**:</span></span>

1. <span data-ttu-id="b098f-131">Válasszon legalább egy 14 napos bejelentkezési előzményei rendelkező fiókkal.</span><span class="sxs-lookup"><span data-stu-id="b098f-131">Choose an account that has at least a 14-day sign-in history.</span></span> 
2. <span data-ttu-id="b098f-132">Módszerekkel:</span><span class="sxs-lookup"><span data-stu-id="b098f-132">Do either:</span></span>
   
   <span data-ttu-id="b098f-133">a.</span><span class="sxs-lookup"><span data-stu-id="b098f-133">a.</span></span> <span data-ttu-id="b098f-134">A VPN-kapcsolattal, miközben lépjen túl[https://myapps.microsoft.com](https://myapps.microsoft.com) hello hitelesítő adataival hello fióknevet, amelyet a toosimulate hello kockázat eseménye.</span><span class="sxs-lookup"><span data-stu-id="b098f-134">While using a VPN, navigate too[https://myapps.microsoft.com](https://myapps.microsoft.com) and enter hello credentials of hello account you want toosimulate hello risk event for.</span></span>
   
   <span data-ttu-id="b098f-135">b.</span><span class="sxs-lookup"><span data-stu-id="b098f-135">b.</span></span> <span data-ttu-id="b098f-136">Kérje meg egy másik helyre toosign hello fiók hitelesítő adataival (nem ajánlott) a társult.</span><span class="sxs-lookup"><span data-stu-id="b098f-136">Ask an associate in a different location toosign in using hello account’s credentials (not recommended).</span></span>

<span data-ttu-id="b098f-137">hello bejelentkezés fog megjelenni a hello Identity Protection-irányítópulton 5 percen belül.</span><span class="sxs-lookup"><span data-stu-id="b098f-137">hello sign-in will show up on hello Identity Protection dashboard within 5 minutes.</span></span>

### <a name="impossible-travel-tooatypical-location"></a><span data-ttu-id="b098f-138">Lehetetlen odautazás tooatypical helye</span><span class="sxs-lookup"><span data-stu-id="b098f-138">Impossible travel tooatypical location</span></span>
<span data-ttu-id="b098f-139">Hello lehetetlen odautazás feltétel szimulálva nem nehéz, mert hello algoritmust használ a machine learning tooweed például lehetetlen odautazás ismerős eszközökről vagy hello directory belüli más felhasználók által használt virtuális magánhálózatok bejelentkezések hamis-tömkelegére kimenő.</span><span class="sxs-lookup"><span data-stu-id="b098f-139">Simulating hello impossible travel condition is difficult because hello algorithm uses machine learning tooweed out false-positives such as impossible travel from familiar devices, or sign-ins from VPNs that are used by other users in hello directory.</span></span> <span data-ttu-id="b098f-140">Ezen felül hello algoritmus igényel 3 too14 nap bejelentkezési előzményeit hello felhasználó kockázati események létrehozásának megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="b098f-140">Additionally, hello algorithm requires a sign-in history of 3 too14 days for hello user before it begins generating risk events.</span></span>

<span data-ttu-id="b098f-141">**toosimulate egy lehetetlen odautazás tooatypical hely, hajtsa végre a lépéseket követve hello**:</span><span class="sxs-lookup"><span data-stu-id="b098f-141">**toosimulate an impossible travel tooatypical location, perform hello following steps**:</span></span>

1. <span data-ttu-id="b098f-142">A szabványos böngészőben nyissa meg a túl[https://myapps.microsoft.com](https://myapps.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="b098f-142">Using your standard browser, navigate too[https://myapps.microsoft.com](https://myapps.microsoft.com).</span></span>  
2. <span data-ttu-id="b098f-143">Adja meg a hello fiók toogenerate lehetetlen odautazás kockázat esemény hello hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="b098f-143">Enter hello credentials of hello account you want toogenerate an impossible travel risk event for.</span></span>
3. <span data-ttu-id="b098f-144">Módosítsa a felhasználói ügynök.</span><span class="sxs-lookup"><span data-stu-id="b098f-144">Change your user agent.</span></span> <span data-ttu-id="b098f-145">Módosítsa az Internet Explorer felhasználói ügynök fejlesztői eszközök, vagy módosítsa a felhasználói ügynök Firefox vagy felhasználói ügynök kapcsoló bővítményével Chrome.</span><span class="sxs-lookup"><span data-stu-id="b098f-145">You can change user agent in Internet Explorer from Developer Tools, or change your user agent in Firefox or Chrome using a user-agent switcher add-on.</span></span>
4. <span data-ttu-id="b098f-146">Az IP-címének módosítása.</span><span class="sxs-lookup"><span data-stu-id="b098f-146">Change your IP address.</span></span> <span data-ttu-id="b098f-147">Az IP-címe a VPN-és a Tor bővítmény használatával, vagy egy új gépet az Azure különböző adatközpont dolgozik módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="b098f-147">You can change your IP address by using a VPN, a Tor add-on, or spinning up a new machine in Azure in a different data center.</span></span>
5. <span data-ttu-id="b098f-148">Bejelentkezés túl[https://myapps.microsoft.com](https://myapps.microsoft.com) használatával hello ugyanazokat a hitelesítő adatokat, mielőtt és hello előző bejelentkezés után néhány percen belül.</span><span class="sxs-lookup"><span data-stu-id="b098f-148">Sign-in too[https://myapps.microsoft.com](https://myapps.microsoft.com) using hello same credentials as before and within a few minutes after hello previous sign-in.</span></span>

<span data-ttu-id="b098f-149">hello bejelentkezés fog megjelenni hello Identity Protection-irányítópult 2 – 4 órán belül.</span><span class="sxs-lookup"><span data-stu-id="b098f-149">hello sign-in will show up in hello Identity Protection dashboard within 2-4 hours.</span></span><br>
<span data-ttu-id="b098f-150">Hello összetett gépi tanulási modellek jár, mert az beszerzése kivétele nem mentése esély van.</span><span class="sxs-lookup"><span data-stu-id="b098f-150">Because of hello complex machine learning models involved, there is a chance it will not get picked up.</span></span><br> <span data-ttu-id="b098f-151">Érdemes lehet tooreplicate ezeket a lépéseket a több Azure AD-fiókok.</span><span class="sxs-lookup"><span data-stu-id="b098f-151">You might want tooreplicate these steps for multiple Azure AD accounts.</span></span>

## <a name="simulating-vulnerabilities"></a><span data-ttu-id="b098f-152">Biztonsági rések szimulálása</span><span class="sxs-lookup"><span data-stu-id="b098f-152">Simulating vulnerabilities</span></span>
<span data-ttu-id="b098f-153">Biztonsági rések egy hibás szereplő is kihasználható az Azure AD környezetben gyengeségei miatt.</span><span class="sxs-lookup"><span data-stu-id="b098f-153">Vulnerabilities are weaknesses in an Azure AD environment that can be exploited by a bad actor.</span></span> <span data-ttu-id="b098f-154">Jelenleg biztonsági rések 3 típusú illesztett az Azure AD Identity Protection használó egyéb szolgáltatásokat az Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b098f-154">Currently 3 types of vulnerabilities are surfaced in Azure AD Identity Protection that leverage other features of Azure AD.</span></span> <span data-ttu-id="b098f-155">A biztonsági rések jelenik meg a hello Identity Protection-irányítópulton automatikusan után ezek a funkciók be vannak állítva.</span><span class="sxs-lookup"><span data-stu-id="b098f-155">These Vulnerabilities will be displayed on hello Identity Protection dashboard automatically once these features are set up.</span></span>

* <span data-ttu-id="b098f-156">Az Azure AD [többtényezős hitelesítés?](../multi-factor-authentication/multi-factor-authentication.md)</span><span class="sxs-lookup"><span data-stu-id="b098f-156">Azure AD [Multi-Factor Authentication?](../multi-factor-authentication/multi-factor-authentication.md)</span></span>
* <span data-ttu-id="b098f-157">Az Azure AD [Cloud App Discovery](active-directory-cloudappdiscovery-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="b098f-157">Azure AD [Cloud App Discovery](active-directory-cloudappdiscovery-whatis.md).</span></span>
* <span data-ttu-id="b098f-158">Az Azure AD [Privileged Identity Management](active-directory-privileged-identity-management-configure.md).</span><span class="sxs-lookup"><span data-stu-id="b098f-158">Azure AD [Privileged Identity Management](active-directory-privileged-identity-management-configure.md).</span></span> 

## <a name="user-compromise-risk"></a><span data-ttu-id="b098f-159">Felhasználó biztonsági sérülés kockázata</span><span class="sxs-lookup"><span data-stu-id="b098f-159">User compromise risk</span></span>
<span data-ttu-id="b098f-160">**tootest felhasználó biztonsági sérülés kockázat, hajtsa végre a lépéseket követve hello**:</span><span class="sxs-lookup"><span data-stu-id="b098f-160">**tootest User compromise risk, perform hello following steps**:</span></span>

1. <span data-ttu-id="b098f-161">Bejelentkezés túl[https://portal.azure.com](https://portal.azure.com) a bérlő globális rendszergazdai hitelesítő adatokkal.</span><span class="sxs-lookup"><span data-stu-id="b098f-161">Sign-in too[https://portal.azure.com](https://portal.azure.com) with global administrator credentials for your tenant.</span></span>
2. <span data-ttu-id="b098f-162">Keresse meg a túl**Identity Protection**.</span><span class="sxs-lookup"><span data-stu-id="b098f-162">Navigate too**Identity Protection**.</span></span> 
3. <span data-ttu-id="b098f-163">A fő hello **Azure AD Identity Protection** panelen kattintson a **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="b098f-163">On hello main **Azure AD Identity Protection** blade, click **Settings**.</span></span> 
4. <span data-ttu-id="b098f-164">A hello **portál beállításait** panelen, a **biztonsági szabályok**, kattintson a **felhasználó biztonsági sérülés kockázati**.</span><span class="sxs-lookup"><span data-stu-id="b098f-164">On hello **Portal Settings** blade, under **Security rules**, click **User compromise risk**.</span></span> 
5. <span data-ttu-id="b098f-165">A hello **kockázat bejelentkezés** panelen kapcsolja **engedélyezése a szabály** ki, és kattintson a **mentése** beállítások.</span><span class="sxs-lookup"><span data-stu-id="b098f-165">On hello **Sign in Risk** blade, turn **Enable rule** off, and then click **Save** settings.</span></span>
6. <span data-ttu-id="b098f-166">Egy adott felhasználói fiók szimulálása egy ismeretlen helyekről vagy névtelen IP-cím kockázat esemény.</span><span class="sxs-lookup"><span data-stu-id="b098f-166">For a given user account, simulate an unfamiliar locations or anonymous IP risk event.</span></span> <span data-ttu-id="b098f-167">Ez fogja jogosultságszint-emelés hello felhasználói kockázati szintjét az adott felhasználó túl**Közepes**.</span><span class="sxs-lookup"><span data-stu-id="b098f-167">This will elevate hello user risk level for that user too**Medium**.</span></span>
7. <span data-ttu-id="b098f-168">Várjon néhány percet, és ellenőrizze, hogy felhasználói szinten, a felhasználó érték **Közepes**.</span><span class="sxs-lookup"><span data-stu-id="b098f-168">Wait a few minutes, and then verify that user level for your user is **Medium**.</span></span>
8. <span data-ttu-id="b098f-169">Nyissa meg toohello **portál beállításait** panelen.</span><span class="sxs-lookup"><span data-stu-id="b098f-169">Go toohello **Portal Settings** blade.</span></span>
9. <span data-ttu-id="b098f-170">A hello **felhasználó biztonsági sérülés kockázati** panel alatt **engedélyezése a szabály**, jelölje be **a** .</span><span class="sxs-lookup"><span data-stu-id="b098f-170">On hello **User Compromise Risk** blade, under **Enable rule**, select **On** .</span></span> 
10. <span data-ttu-id="b098f-171">Válasszon egyet az alábbi beállítások hello:</span><span class="sxs-lookup"><span data-stu-id="b098f-171">Select one of hello following options:</span></span>
    
    <span data-ttu-id="b098f-172">a.</span><span class="sxs-lookup"><span data-stu-id="b098f-172">a.</span></span> <span data-ttu-id="b098f-173">tooblock, jelölje be **Közepes** alatt **blokk bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="b098f-173">tooblock, select **Medium** under **Block sign in**.</span></span>
    
    <span data-ttu-id="b098f-174">b.</span><span class="sxs-lookup"><span data-stu-id="b098f-174">b.</span></span> <span data-ttu-id="b098f-175">tooenforce biztonságos jelszó módosítása, jelölje be **Közepes** alatt **többtényezős hitelesítést**.</span><span class="sxs-lookup"><span data-stu-id="b098f-175">tooenforce secure password change, select **Medium** under **Require multi-factor authentication**.</span></span>
11. <span data-ttu-id="b098f-176">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="b098f-176">Click **Save**.</span></span>
12. <span data-ttu-id="b098f-177">Kockázati-alapú feltételes hozzáférés egy felhasználó használata egy emelt szintű kockázati szintjét a bejelentkezéssel most tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="b098f-177">You can now test risk-based conditional access by signing in using a user with an elevated risk level.</span></span> <span data-ttu-id="b098f-178">Ha hello felhasználói kockázat közepes, attól függően, hogy hello konfigurációs házirend, a bejelentkezés vagy letiltása, vagy toochange kényszerítve vannak a jelszavát.</span><span class="sxs-lookup"><span data-stu-id="b098f-178">If hello user risk is Medium, depending on hello configuration of your policy, your sign-in is be either blocked or you are forced toochange your password.</span></span> 
    <br><br><span data-ttu-id="b098f-179">
    ![Alkalmazástervezési](./media/active-directory-identityprotection-playbook/201.png "forgatókönyv")
    </span><span class="sxs-lookup"><span data-stu-id="b098f-179">
![Playbook](./media/active-directory-identityprotection-playbook/201.png "Playbook")
</span></span><br>

## <a name="sign-in-risk"></a><span data-ttu-id="b098f-180">Bejelentkezési kockázata</span><span class="sxs-lookup"><span data-stu-id="b098f-180">Sign-in risk</span></span>
<span data-ttu-id="b098f-181">**tootest egy bejelentkezési kockázat, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="b098f-181">**tootest a sign in risk, perform hello following steps:**</span></span>

1. <span data-ttu-id="b098f-182">Bejelentkezés túl[https://portal.azure.com ](https://portal.azure.com) a bérlő globális rendszergazdai hitelesítő adatokkal.</span><span class="sxs-lookup"><span data-stu-id="b098f-182">Sign-in too[https://portal.azure.com ](https://portal.azure.com) with global administrator credentials for your tenant.</span></span>
2. <span data-ttu-id="b098f-183">Keresse meg a túl**Identity Protection**.</span><span class="sxs-lookup"><span data-stu-id="b098f-183">Navigate too**Identity Protection**.</span></span>
3. <span data-ttu-id="b098f-184">A fő hello **Azure AD Identity Protection** panelen kattintson a **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="b098f-184">On hello main **Azure AD Identity Protection** blade, click **Settings**.</span></span> 
4. <span data-ttu-id="b098f-185">A hello **portál beállításait** panelen, a **biztonsági szabályok**, kattintson a **kockázat bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="b098f-185">On hello **Portal Settings** blade, under **Security rules**, click **Sign in risk**.</span></span>
5. <span data-ttu-id="b098f-186">A hello ** kockázat bejelentkezés ** panelen válassza **a** alatt **engedélyezése a szabály**.</span><span class="sxs-lookup"><span data-stu-id="b098f-186">On hello **Sign in Risk **blade, select **On** under **Enable rule**.</span></span> 
6. <span data-ttu-id="b098f-187">Válasszon egyet az alábbi beállítások hello:</span><span class="sxs-lookup"><span data-stu-id="b098f-187">Select one of hello following options:</span></span>
   
   <span data-ttu-id="b098f-188">a.</span><span class="sxs-lookup"><span data-stu-id="b098f-188">a.</span></span> <span data-ttu-id="b098f-189">tooblock, jelölje be **Közepes** alatt **blokk jelentkezzen be**</span><span class="sxs-lookup"><span data-stu-id="b098f-189">tooblock, select **Medium** under **Block sign in**</span></span>
   
   <span data-ttu-id="b098f-190">b.</span><span class="sxs-lookup"><span data-stu-id="b098f-190">b.</span></span> <span data-ttu-id="b098f-191">tooenforce biztonságos jelszó módosítása, jelölje be **Közepes** alatt **többtényezős hitelesítést**.</span><span class="sxs-lookup"><span data-stu-id="b098f-191">tooenforce secure password change, select **Medium** under **Require multi-factor authentication**.</span></span>
7. <span data-ttu-id="b098f-192">tooblock, a blokk válassza közepes jelentkezzen be.</span><span class="sxs-lookup"><span data-stu-id="b098f-192">tooblock, select Medium under Block sign in.</span></span>
8. <span data-ttu-id="b098f-193">tooenforce többtényezős hitelesítést, jelölje be **Közepes** alatt **többtényezős hitelesítést**.</span><span class="sxs-lookup"><span data-stu-id="b098f-193">tooenforce multi-factor authentication, select **Medium** under **Require multi-factor authentication**.</span></span>
9. <span data-ttu-id="b098f-194">Kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="b098f-194">Click on **Save**.</span></span>
10. <span data-ttu-id="b098f-195">Kockázati-alapú feltételes hozzáférés hello ismeretlen helyek szimulál most tesztelheti, vagy névtelen IP kockázati események, mivel azok is **Közepes** események kockázatát.</span><span class="sxs-lookup"><span data-stu-id="b098f-195">You can now test risk-based conditional access by simulating hello unfamiliar locations or anonymous IP risk events because they are both **Medium** risk events.</span></span>


<span data-ttu-id="b098f-196">![Alkalmazástervezési](./media/active-directory-identityprotection-playbook/200.png "forgatókönyv")</span><span class="sxs-lookup"><span data-stu-id="b098f-196">![Playbook](./media/active-directory-identityprotection-playbook/200.png "Playbook")</span></span>


## <a name="see-also"></a><span data-ttu-id="b098f-197">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="b098f-197">See also</span></span>
* [<span data-ttu-id="b098f-198">Az Azure Active Directory azonosító adatok védelmét</span><span class="sxs-lookup"><span data-stu-id="b098f-198">Azure Active Directory Identity Protection</span></span>](active-directory-identityprotection.md)

