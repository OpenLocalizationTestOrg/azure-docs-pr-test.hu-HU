---
title: "Az Azure Active Directory Identity Protection-forgatókönyv |} Microsoft Docs"
description: "Ismerje meg, az Azure AD Identity Protection miként korlátozhatja, hogy a támadó kihasználni a sérült biztonságú identitás vagy az eszköz és identitás vagy egy eszköz, amely korábban gyanús vagy megsértik ismert biztonságossá tételéhez."
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
ms.openlocfilehash: 2ecd07faed785fa6aa179ac1cca35a70d965e1dc
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="azure-active-directory-identity-protection-playbook"></a><span data-ttu-id="3151b-104">Az Azure Active Directory Identity Protection-forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="3151b-104">Azure Active Directory Identity Protection playbook</span></span>
<span data-ttu-id="3151b-105">Ez a forgatókönyv segítséget:</span><span class="sxs-lookup"><span data-stu-id="3151b-105">This playbook helps you to:</span></span>

* <span data-ttu-id="3151b-106">Az Identity Protection környezetben adatok feltöltése a szimuláció kockázati eseményekről és biztonsági rések</span><span class="sxs-lookup"><span data-stu-id="3151b-106">Populate data in the Identity Protection environment by simulating risk events and vulnerabilities</span></span>
* <span data-ttu-id="3151b-107">Kockázati-alapú feltételes hozzáférés szabályzatainak beállítása, és ezek a házirendek hatásának tesztelése</span><span class="sxs-lookup"><span data-stu-id="3151b-107">Set up risk-based conditional access policies and test the impact of these policies</span></span>

## <a name="simulating-risk-events"></a><span data-ttu-id="3151b-108">Kockázati események szimulálása</span><span class="sxs-lookup"><span data-stu-id="3151b-108">Simulating Risk Events</span></span>
<span data-ttu-id="3151b-109">Ez a szakasz biztosít lépéseket a következő kockázatok eseménytípusok szimulálva:</span><span class="sxs-lookup"><span data-stu-id="3151b-109">This section provides you with steps for simulating the following risk event types:</span></span>

* <span data-ttu-id="3151b-110">Bejelentkezések névtelen IP-címről (egyszerű)</span><span class="sxs-lookup"><span data-stu-id="3151b-110">Sign-ins from anonymous IP addresses (easy)</span></span>
* <span data-ttu-id="3151b-111">Bejelentkezések ismeretlen helyekről (közepes)</span><span class="sxs-lookup"><span data-stu-id="3151b-111">Sign-ins from unfamiliar locations (moderate)</span></span>
* <span data-ttu-id="3151b-112">Lehetetlen odautazás bejelentkezés szokatlan helyekről (nehéz)</span><span class="sxs-lookup"><span data-stu-id="3151b-112">Impossible travel to atypical locations (difficult)</span></span>

<span data-ttu-id="3151b-113">Más kockázati események nem szimulált, biztonságos módon.</span><span class="sxs-lookup"><span data-stu-id="3151b-113">Other risk events cannot be simulated in a secure manner.</span></span>

### <a name="sign-ins-from-anonymous-ip-addresses"></a><span data-ttu-id="3151b-114">Névtelen IP-címről történő bejelentkezések</span><span class="sxs-lookup"><span data-stu-id="3151b-114">Sign-ins from anonymous IP addresses</span></span>
<span data-ttu-id="3151b-115">A kockázat esemény a névtelen proxy IP-címként azonosított IP-cím sikeresen bejelentkezett felhasználók azonosítja.</span><span class="sxs-lookup"><span data-stu-id="3151b-115">This risk event type identifies users who have successfully signed in from an IP address that has been identified as an anonymous proxy IP address.</span></span> <span data-ttu-id="3151b-116">Ezek a proxyk szeretné az eszköz IP-címet, és a rosszindulatú behatolással szemben használható személyek által használt.</span><span class="sxs-lookup"><span data-stu-id="3151b-116">These proxies are used by people who want to hide their device’s IP address and may be used for malicious intent.</span></span>

<span data-ttu-id="3151b-117">**Szimulálása a bejelentkezés egy névtelen IP-címről, a következő lépésekkel**:</span><span class="sxs-lookup"><span data-stu-id="3151b-117">**To simulate a sign-in from an anonymous IP, perform the following steps**:</span></span>

1. <span data-ttu-id="3151b-118">Töltse le a [Tor böngésző](https://www.torproject.org/projects/torbrowser.html.en).</span><span class="sxs-lookup"><span data-stu-id="3151b-118">Download the [Tor Browser](https://www.torproject.org/projects/torbrowser.html.en).</span></span>
2. <span data-ttu-id="3151b-119">Keresse meg a Tor böngészővel [https://myapps.microsoft.com](https://myapps.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="3151b-119">Using the Tor Browser, navigate to [https://myapps.microsoft.com](https://myapps.microsoft.com).</span></span>   
3. <span data-ttu-id="3151b-120">Adja meg a fiók, akkor jelenik meg a hitelesítő adatait a **névtelen IP-címekről bejelentkezések** jelentés.</span><span class="sxs-lookup"><span data-stu-id="3151b-120">Enter the credentials of the account you want to appear in the **Sign-ins from anonymous IP addresses** report.</span></span>

<span data-ttu-id="3151b-121">A bejelentkezés jelennek meg az Identity Protection-irányítópult 5 percen belül.</span><span class="sxs-lookup"><span data-stu-id="3151b-121">The sign-in will show up on the Identity Protection dashboard within 5 minutes.</span></span> 

### <a name="sign-ins-from-unfamiliar-locations"></a><span data-ttu-id="3151b-122">Ismeretlen helyekről történt bejelentkezések</span><span class="sxs-lookup"><span data-stu-id="3151b-122">Sign-ins from unfamiliar locations</span></span>
<span data-ttu-id="3151b-123">Az ismeretlen helyek kockázat egy valós idejű bejelentkezési értékelési mechanizmus, amely a korábbi bejelentkezési helyek figyelembe veszi (szélesség, IP / hosszúság és a ASN) új / ismeretlen helyek meghatározásához.</span><span class="sxs-lookup"><span data-stu-id="3151b-123">The unfamiliar locations risk is a real-time sign-in evaluation mechanism that considers past sign-in locations (IP, Latitude / Longitude and ASN) to determine new / unfamiliar locations.</span></span> <span data-ttu-id="3151b-124">A rendszer tárolja a korábbi IP-címek, szélesség / hosszúság, és a ASN-eket, hogy egy felhasználó és figyelembe veszi azokat a megszokott helyek lehet.</span><span class="sxs-lookup"><span data-stu-id="3151b-124">The system stores previous IPs, Latitude / Longitude, and ASNs of a user and considers these to be familiar locations.</span></span> <span data-ttu-id="3151b-125">A bejelentkezési helye akkor tekinthető ismeri, ha a bejelentkezési helye nem egyezik a meglévő ismerős helyeken.</span><span class="sxs-lookup"><span data-stu-id="3151b-125">A sign-in location is considered unfamiliar if the sign-in location does not match any of the existing familiar locations.</span></span>

<span data-ttu-id="3151b-126">Az Azure Active Directory azonosító adatok védelmét:</span><span class="sxs-lookup"><span data-stu-id="3151b-126">Azure Active Directory Identity Protection:</span></span>  

* <span data-ttu-id="3151b-127">kezdeti tanulási időszaka 14 nap során, ami azt nem ez a jelző bármely új helyek ismeretlen helyként.</span><span class="sxs-lookup"><span data-stu-id="3151b-127">has an initial learning period of 14 days during which it does not flag any new locations as unfamiliar locations.</span></span>
* <span data-ttu-id="3151b-128">figyelmen kívül hagyja az ismerős eszközöket és a meglévő ismerős hely földrajzilag megközelítik helyek bejelentkezések.</span><span class="sxs-lookup"><span data-stu-id="3151b-128">ignores sign-ins from familiar devices and locations that are geographically close to an existing familiar location.</span></span>

<span data-ttu-id="3151b-129">Szimulálása ismeretlen helyeket, hogy jelentkezzen be egy helyről, és az eszköz, amely a fiók még nem jelentkezett be az előtt.</span><span class="sxs-lookup"><span data-stu-id="3151b-129">To simulate unfamiliar locations, you have to sign in from a location and device that the account has not signed in from before.</span></span> 

<span data-ttu-id="3151b-130">**Szimulálása a bejelentkezés egy ismeretlen helyről, a következő lépésekkel**:</span><span class="sxs-lookup"><span data-stu-id="3151b-130">**To simulate a sign-in from an unfamiliar location, perform the following steps**:</span></span>

1. <span data-ttu-id="3151b-131">Válasszon legalább egy 14 napos bejelentkezési előzményei rendelkező fiókkal.</span><span class="sxs-lookup"><span data-stu-id="3151b-131">Choose an account that has at least a 14-day sign-in history.</span></span> 
2. <span data-ttu-id="3151b-132">Módszerekkel:</span><span class="sxs-lookup"><span data-stu-id="3151b-132">Do either:</span></span>
   
   <span data-ttu-id="3151b-133">a.</span><span class="sxs-lookup"><span data-stu-id="3151b-133">a.</span></span> <span data-ttu-id="3151b-134">A VPN-en, navigáljon [https://myapps.microsoft.com](https://myapps.microsoft.com) , és írja be a szeretné szimulálni a kockázat esemény fiók hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="3151b-134">While using a VPN, navigate to [https://myapps.microsoft.com](https://myapps.microsoft.com) and enter the credentials of the account you want to simulate the risk event for.</span></span>
   
   <span data-ttu-id="3151b-135">b.</span><span class="sxs-lookup"><span data-stu-id="3151b-135">b.</span></span> <span data-ttu-id="3151b-136">Kérje meg egy társítható, egy másik helyen is bejelentkezhet a fiók hitelesítő adatait (nem ajánlott).</span><span class="sxs-lookup"><span data-stu-id="3151b-136">Ask an associate in a different location to sign in using the account’s credentials (not recommended).</span></span>

<span data-ttu-id="3151b-137">A bejelentkezés jelennek meg az Identity Protection-irányítópult 5 percen belül.</span><span class="sxs-lookup"><span data-stu-id="3151b-137">The sign-in will show up on the Identity Protection dashboard within 5 minutes.</span></span>

### <a name="impossible-travel-to-atypical-location"></a><span data-ttu-id="3151b-138">Lehetetlen odautazás alapján ezúttal szokatlan helyre</span><span class="sxs-lookup"><span data-stu-id="3151b-138">Impossible travel to atypical location</span></span>
<span data-ttu-id="3151b-139">Lehetetlen odautazás feltétel szimulálva nem nehéz, mert az algoritmus által használt gépi tanulással például lehetetlen odautazás ismerős eszközökről vagy a virtuális magánhálózatok, hogy a címtárban más felhasználók által használt bejelentkezések hamis-tömkelegére kimenő gyomláláskor.</span><span class="sxs-lookup"><span data-stu-id="3151b-139">Simulating the impossible travel condition is difficult because the algorithm uses machine learning to weed out false-positives such as impossible travel from familiar devices, or sign-ins from VPNs that are used by other users in the directory.</span></span> <span data-ttu-id="3151b-140">Ezen felül az algoritmus igényel 3 – 14 nap bejelentkezési előzményeit a felhasználó kockázati események létrehozásának megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="3151b-140">Additionally, the algorithm requires a sign-in history of 3 to 14 days for the user before it begins generating risk events.</span></span>

<span data-ttu-id="3151b-141">**Ezzel szimulálva egy lehetetlen odautazás alapján ezúttal szokatlan helyre, a következő lépésekkel**:</span><span class="sxs-lookup"><span data-stu-id="3151b-141">**To simulate an impossible travel to atypical location, perform the following steps**:</span></span>

1. <span data-ttu-id="3151b-142">Keresse meg a szabványos böngészővel [https://myapps.microsoft.com](https://myapps.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="3151b-142">Using your standard browser, navigate to [https://myapps.microsoft.com](https://myapps.microsoft.com).</span></span>  
2. <span data-ttu-id="3151b-143">Adjon meg egy lehetetlen odautazás kockázat eseményt generál a fiók hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="3151b-143">Enter the credentials of the account you want to generate an impossible travel risk event for.</span></span>
3. <span data-ttu-id="3151b-144">Módosítsa a felhasználói ügynök.</span><span class="sxs-lookup"><span data-stu-id="3151b-144">Change your user agent.</span></span> <span data-ttu-id="3151b-145">Módosítsa az Internet Explorer felhasználói ügynök fejlesztői eszközök, vagy módosítsa a felhasználói ügynök Firefox vagy felhasználói ügynök kapcsoló bővítményével Chrome.</span><span class="sxs-lookup"><span data-stu-id="3151b-145">You can change user agent in Internet Explorer from Developer Tools, or change your user agent in Firefox or Chrome using a user-agent switcher add-on.</span></span>
4. <span data-ttu-id="3151b-146">Az IP-címének módosítása.</span><span class="sxs-lookup"><span data-stu-id="3151b-146">Change your IP address.</span></span> <span data-ttu-id="3151b-147">Az IP-címe a VPN-és a Tor bővítmény használatával, vagy egy új gépet az Azure különböző adatközpont dolgozik módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="3151b-147">You can change your IP address by using a VPN, a Tor add-on, or spinning up a new machine in Azure in a different data center.</span></span>
5. <span data-ttu-id="3151b-148">Bejelentkezés az [https://myapps.microsoft.com](https://myapps.microsoft.com) hitelesítő adatokkal, mielőtt, és a korábbi bejelentkezés után néhány percen belül.</span><span class="sxs-lookup"><span data-stu-id="3151b-148">Sign-in to [https://myapps.microsoft.com](https://myapps.microsoft.com) using the same credentials as before and within a few minutes after the previous sign-in.</span></span>

<span data-ttu-id="3151b-149">A bejelentkezés fog megjelenni az Identity Protection-irányítópult 2 – 4 órán belül.</span><span class="sxs-lookup"><span data-stu-id="3151b-149">The sign-in will show up in the Identity Protection dashboard within 2-4 hours.</span></span><br>
<span data-ttu-id="3151b-150">Összetett gépi tanulási modellek jár, mert az beszerzése kivétele nem mentése esély van.</span><span class="sxs-lookup"><span data-stu-id="3151b-150">Because of the complex machine learning models involved, there is a chance it will not get picked up.</span></span><br> <span data-ttu-id="3151b-151">Előfordulhat, hogy szeretné ezeket a lépéseket, a több Azure AD-fiókok replikálása.</span><span class="sxs-lookup"><span data-stu-id="3151b-151">You might want to replicate these steps for multiple Azure AD accounts.</span></span>

## <a name="simulating-vulnerabilities"></a><span data-ttu-id="3151b-152">Biztonsági rések szimulálása</span><span class="sxs-lookup"><span data-stu-id="3151b-152">Simulating vulnerabilities</span></span>
<span data-ttu-id="3151b-153">Biztonsági rések egy hibás szereplő is kihasználható az Azure AD környezetben gyengeségei miatt.</span><span class="sxs-lookup"><span data-stu-id="3151b-153">Vulnerabilities are weaknesses in an Azure AD environment that can be exploited by a bad actor.</span></span> <span data-ttu-id="3151b-154">Jelenleg biztonsági rések 3 típusú illesztett az Azure AD Identity Protection használó egyéb szolgáltatásokat az Azure AD.</span><span class="sxs-lookup"><span data-stu-id="3151b-154">Currently 3 types of vulnerabilities are surfaced in Azure AD Identity Protection that leverage other features of Azure AD.</span></span> <span data-ttu-id="3151b-155">A biztonsági rések jelenik meg az Identity Protection-Irányítópult automatikusan után ezek a funkciók be vannak állítva.</span><span class="sxs-lookup"><span data-stu-id="3151b-155">These Vulnerabilities will be displayed on the Identity Protection dashboard automatically once these features are set up.</span></span>

* <span data-ttu-id="3151b-156">Az Azure AD [többtényezős hitelesítés?](../multi-factor-authentication/multi-factor-authentication.md)</span><span class="sxs-lookup"><span data-stu-id="3151b-156">Azure AD [Multi-Factor Authentication?](../multi-factor-authentication/multi-factor-authentication.md)</span></span>
* <span data-ttu-id="3151b-157">Az Azure AD [Cloud App Discovery](active-directory-cloudappdiscovery-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="3151b-157">Azure AD [Cloud App Discovery](active-directory-cloudappdiscovery-whatis.md).</span></span>
* <span data-ttu-id="3151b-158">Az Azure AD [Privileged Identity Management](active-directory-privileged-identity-management-configure.md).</span><span class="sxs-lookup"><span data-stu-id="3151b-158">Azure AD [Privileged Identity Management](active-directory-privileged-identity-management-configure.md).</span></span> 

## <a name="user-compromise-risk"></a><span data-ttu-id="3151b-159">Felhasználó biztonsági sérülés kockázata</span><span class="sxs-lookup"><span data-stu-id="3151b-159">User compromise risk</span></span>
<span data-ttu-id="3151b-160">**Felhasználó biztonsági sérülés kockázat, hajtsa végre a következő lépéseket**:</span><span class="sxs-lookup"><span data-stu-id="3151b-160">**To test User compromise risk, perform the following steps**:</span></span>

1. <span data-ttu-id="3151b-161">Bejelentkezés az [https://portal.azure.com](https://portal.azure.com) a bérlő globális rendszergazdai hitelesítő adatokkal.</span><span class="sxs-lookup"><span data-stu-id="3151b-161">Sign-in to [https://portal.azure.com](https://portal.azure.com) with global administrator credentials for your tenant.</span></span>
2. <span data-ttu-id="3151b-162">Navigáljon a **Identity Protection**.</span><span class="sxs-lookup"><span data-stu-id="3151b-162">Navigate to **Identity Protection**.</span></span> 
3. <span data-ttu-id="3151b-163">A fő **Azure AD Identity Protection** panelen kattintson a **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="3151b-163">On the main **Azure AD Identity Protection** blade, click **Settings**.</span></span> 
4. <span data-ttu-id="3151b-164">A a **portál beállításait** panelen, a **biztonsági szabályok**, kattintson a **felhasználó biztonsági sérülés kockázati**.</span><span class="sxs-lookup"><span data-stu-id="3151b-164">On the **Portal Settings** blade, under **Security rules**, click **User compromise risk**.</span></span> 
5. <span data-ttu-id="3151b-165">A a **kockázat bejelentkezés** panelen kapcsolja **engedélyezése a szabály** ki, és kattintson a **mentése** beállításait.</span><span class="sxs-lookup"><span data-stu-id="3151b-165">On the **Sign in Risk** blade, turn **Enable rule** off, and then click **Save** settings.</span></span>
6. <span data-ttu-id="3151b-166">Egy adott felhasználói fiók szimulálása egy ismeretlen helyekről vagy névtelen IP-cím kockázat esemény.</span><span class="sxs-lookup"><span data-stu-id="3151b-166">For a given user account, simulate an unfamiliar locations or anonymous IP risk event.</span></span> <span data-ttu-id="3151b-167">Ez fogja jogosultságszint-emelés, hogy a felhasználó felhasználói kockázati szintjének **Közepes**.</span><span class="sxs-lookup"><span data-stu-id="3151b-167">This will elevate the user risk level for that user to **Medium**.</span></span>
7. <span data-ttu-id="3151b-168">Várjon néhány percet, és ellenőrizze, hogy felhasználói szinten, a felhasználó érték **Közepes**.</span><span class="sxs-lookup"><span data-stu-id="3151b-168">Wait a few minutes, and then verify that user level for your user is **Medium**.</span></span>
8. <span data-ttu-id="3151b-169">Lépjen a **portál beállításait** panelen.</span><span class="sxs-lookup"><span data-stu-id="3151b-169">Go to the **Portal Settings** blade.</span></span>
9. <span data-ttu-id="3151b-170">A a **felhasználó biztonsági sérülés kockázati** panel alatt **engedélyezése a szabály**, jelölje be **a** .</span><span class="sxs-lookup"><span data-stu-id="3151b-170">On the **User Compromise Risk** blade, under **Enable rule**, select **On** .</span></span> 
10. <span data-ttu-id="3151b-171">A következő lehetőségek közül:</span><span class="sxs-lookup"><span data-stu-id="3151b-171">Select one of the following options:</span></span>
    
    <span data-ttu-id="3151b-172">a.</span><span class="sxs-lookup"><span data-stu-id="3151b-172">a.</span></span> <span data-ttu-id="3151b-173">Blokk, jelölje be a **Közepes** alatt **blokk bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="3151b-173">To block, select **Medium** under **Block sign in**.</span></span>
    
    <span data-ttu-id="3151b-174">b.</span><span class="sxs-lookup"><span data-stu-id="3151b-174">b.</span></span> <span data-ttu-id="3151b-175">Biztonságos jelszó módosítása megköveteléséhez **Közepes** alatt **többtényezős hitelesítést**.</span><span class="sxs-lookup"><span data-stu-id="3151b-175">To enforce secure password change, select **Medium** under **Require multi-factor authentication**.</span></span>
11. <span data-ttu-id="3151b-176">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="3151b-176">Click **Save**.</span></span>
12. <span data-ttu-id="3151b-177">Kockázati-alapú feltételes hozzáférés egy felhasználó használata egy emelt szintű kockázati szintjét a bejelentkezéssel most tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="3151b-177">You can now test risk-based conditional access by signing in using a user with an elevated risk level.</span></span> <span data-ttu-id="3151b-178">Ha a felhasználó kockázati közepes, attól függően, hogy a házirend konfigurációját, a bejelentkezés vagy letiltása, vagy hogy kényszerítve vannak-e a jelszó módosítása.</span><span class="sxs-lookup"><span data-stu-id="3151b-178">If the user risk is Medium, depending on the configuration of your policy, your sign-in is be either blocked or you are forced to change your password.</span></span> 
    <br><br><span data-ttu-id="3151b-179">
    ![Alkalmazástervezési](./media/active-directory-identityprotection-playbook/201.png "forgatókönyv")
    </span><span class="sxs-lookup"><span data-stu-id="3151b-179">
![Playbook](./media/active-directory-identityprotection-playbook/201.png "Playbook")
</span></span><br>

## <a name="sign-in-risk"></a><span data-ttu-id="3151b-180">Bejelentkezési kockázata</span><span class="sxs-lookup"><span data-stu-id="3151b-180">Sign-in risk</span></span>
<span data-ttu-id="3151b-181">**A bejelentkezési kockázatot, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="3151b-181">**To test a sign in risk, perform the following steps:**</span></span>

1. <span data-ttu-id="3151b-182">Bejelentkezés az [https://portal.azure.com ](https://portal.azure.com) a bérlő globális rendszergazdai hitelesítő adatokkal.</span><span class="sxs-lookup"><span data-stu-id="3151b-182">Sign-in to [https://portal.azure.com ](https://portal.azure.com) with global administrator credentials for your tenant.</span></span>
2. <span data-ttu-id="3151b-183">Navigáljon a **Identity Protection**.</span><span class="sxs-lookup"><span data-stu-id="3151b-183">Navigate to **Identity Protection**.</span></span>
3. <span data-ttu-id="3151b-184">A fő **Azure AD Identity Protection** panelen kattintson a **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="3151b-184">On the main **Azure AD Identity Protection** blade, click **Settings**.</span></span> 
4. <span data-ttu-id="3151b-185">Az a **portál beállításait** panelen, a **biztonsági szabályok**, kattintson a **kockázat bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="3151b-185">On the **Portal Settings** blade, under **Security rules**, click **Sign in risk**.</span></span>
5. <span data-ttu-id="3151b-186">Az a ** kockázat bejelentkezés ** panelen válassza **a** alatt **engedélyezése a szabály**.</span><span class="sxs-lookup"><span data-stu-id="3151b-186">On the **Sign in Risk **blade, select **On** under **Enable rule**.</span></span> 
6. <span data-ttu-id="3151b-187">A következő lehetőségek közül:</span><span class="sxs-lookup"><span data-stu-id="3151b-187">Select one of the following options:</span></span>
   
   <span data-ttu-id="3151b-188">a.</span><span class="sxs-lookup"><span data-stu-id="3151b-188">a.</span></span> <span data-ttu-id="3151b-189">Letilthatja, válassza ki a **Közepes** alatt **blokk jelentkezzen be**</span><span class="sxs-lookup"><span data-stu-id="3151b-189">To block, select **Medium** under **Block sign in**</span></span>
   
   <span data-ttu-id="3151b-190">b.</span><span class="sxs-lookup"><span data-stu-id="3151b-190">b.</span></span> <span data-ttu-id="3151b-191">Biztonságos jelszó módosítása megköveteléséhez **Közepes** alatt **többtényezős hitelesítést**.</span><span class="sxs-lookup"><span data-stu-id="3151b-191">To enforce secure password change, select **Medium** under **Require multi-factor authentication**.</span></span>
7. <span data-ttu-id="3151b-192">Blokkolja, válassza ki a közepes blokk bejelentkezés alatt.</span><span class="sxs-lookup"><span data-stu-id="3151b-192">To block, select Medium under Block sign in.</span></span>
8. <span data-ttu-id="3151b-193">Többtényezős hitelesítés megköveteléséhez **Közepes** alatt **többtényezős hitelesítést**.</span><span class="sxs-lookup"><span data-stu-id="3151b-193">To enforce multi-factor authentication, select **Medium** under **Require multi-factor authentication**.</span></span>
9. <span data-ttu-id="3151b-194">Kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="3151b-194">Click on **Save**.</span></span>
10. <span data-ttu-id="3151b-195">Az ismeretlen helyek szimulál most tesztelheti kockázat-alapú feltételes hozzáférés, vagy névtelen IP kockázati események, mivel azok is **Közepes** események kockázatát.</span><span class="sxs-lookup"><span data-stu-id="3151b-195">You can now test risk-based conditional access by simulating the unfamiliar locations or anonymous IP risk events because they are both **Medium** risk events.</span></span>


<span data-ttu-id="3151b-196">![Alkalmazástervezési](./media/active-directory-identityprotection-playbook/200.png "forgatókönyv")</span><span class="sxs-lookup"><span data-stu-id="3151b-196">![Playbook](./media/active-directory-identityprotection-playbook/200.png "Playbook")</span></span>


## <a name="see-also"></a><span data-ttu-id="3151b-197">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="3151b-197">See also</span></span>
* [<span data-ttu-id="3151b-198">Az Azure Active Directory azonosító adatok védelmét</span><span class="sxs-lookup"><span data-stu-id="3151b-198">Azure Active Directory Identity Protection</span></span>](active-directory-identityprotection.md)

