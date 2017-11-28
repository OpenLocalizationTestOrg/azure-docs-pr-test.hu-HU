---
title: "Az Azure AD Connect: Zökkenőmentes egyszeri bejelentkezés – első lépések |} Microsoft Docs"
description: "A cikkből megtudhatja, hogyan az Azure Active Directory zökkenőmentes egyszeri bejelentkezést lásson."
services: active-directory
keywords: "Mi az Azure AD Connect telepítés Active Directory szükséges összetevőket az Azure AD, SSO, egyszeri bejelentkezést."
documentationcenter: 
author: swkrish
manager: femila
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/04/2017
ms.author: billmath
ms.openlocfilehash: 977108687734a5eb7f7a30419de2a6bdef184d0e
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="azure-active-directory-seamless-single-sign-on-quick-start"></a><span data-ttu-id="aae1d-104">Az Azure Active Directory zökkenőmentes egyszeri bejelentkezés: első lépések</span><span class="sxs-lookup"><span data-stu-id="aae1d-104">Azure Active Directory Seamless Single Sign-On: Quick start</span></span>

## <a name="how-to-deploy-seamless-sso"></a><span data-ttu-id="aae1d-105">Zökkenőmentes SSO központi telepítése</span><span class="sxs-lookup"><span data-stu-id="aae1d-105">How to deploy Seamless SSO</span></span>

<span data-ttu-id="aae1d-106">Azure Active Directory zökkenőmentes egyszeri bejelentkezést (az Azure AD zökkenőmentes SSO) amikor kapcsolódnak a vállalati asztali számítógép kapcsolódik a vállalati hálózathoz a felhasználók bejelentkezésekor automatikusan.</span><span class="sxs-lookup"><span data-stu-id="aae1d-106">Azure Active Directory Seamless Single Sign-On (Azure AD Seamless SSO) automatically signs users in when they are on their corporate desktops connected to your corporate network.</span></span> <span data-ttu-id="aae1d-107">A felhasználók egyszerű hozzáférést biztosít a felhőalapú alkalmazások anélkül, hogy semmilyen további helyszíni összetevőt.</span><span class="sxs-lookup"><span data-stu-id="aae1d-107">It provides your users easy access to your cloud-based applications without needing any additional on-premises components.</span></span>

>[!IMPORTANT]
><span data-ttu-id="aae1d-108">A zökkenőmentes SSO-funkció jelenleg előzetes verzió.</span><span class="sxs-lookup"><span data-stu-id="aae1d-108">The Seamless SSO feature is currently in preview.</span></span>

<span data-ttu-id="aae1d-109">Zökkenőmentes SSO telepítéséhez kövesse az alábbi lépéseket kell:</span><span class="sxs-lookup"><span data-stu-id="aae1d-109">To deploy Seamless SSO, you need to follow these steps:</span></span>

## <a name="step-1-check-prerequisites"></a><span data-ttu-id="aae1d-110">1. lépés: Követelmények ellenőrzésének megismétlése</span><span class="sxs-lookup"><span data-stu-id="aae1d-110">Step 1: Check prerequisites</span></span>

<span data-ttu-id="aae1d-111">Győződjön meg arról, hogy a következő előfeltételeket:</span><span class="sxs-lookup"><span data-stu-id="aae1d-111">Ensure that the following prerequisites are in place:</span></span>

1. <span data-ttu-id="aae1d-112">Az Azure AD Connect-kiszolgáló beállítása: Ha [áteresztő hitelesítés](active-directory-aadconnect-pass-through-authentication.md) legyen a bejelentkezési módszer semmilyen további műveletet nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="aae1d-112">Set up your Azure AD Connect server: If you use [Pass-through Authentication](active-directory-aadconnect-pass-through-authentication.md) as your sign-in method, no further action is required.</span></span> <span data-ttu-id="aae1d-113">Ha [Jelszókivonat-szinkronizálást](active-directory-aadconnectsync-implement-password-synchronization.md) legyen a bejelentkezési módszer, és ha az Azure AD Connect és az Azure AD között tűzfal, ellenőrizze, hogy:</span><span class="sxs-lookup"><span data-stu-id="aae1d-113">If you use [Password Hash Synchronization](active-directory-aadconnectsync-implement-password-synchronization.md) as your sign-in method, and if there is a firewall between Azure AD Connect and Azure AD, ensure that:</span></span>
- <span data-ttu-id="aae1d-114">Verziók 1.1.484.0 használ vagy újabb, az Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="aae1d-114">You are using versions 1.1.484.0 or later of Azure AD Connect.</span></span>
- <span data-ttu-id="aae1d-115">Az Azure AD Connect kommunikálhatnak `*.msappproxy.net` URL-címek és a 443-as porton alatt.</span><span class="sxs-lookup"><span data-stu-id="aae1d-115">Azure AD Connect can communicate with `*.msappproxy.net` URLs and over port 443.</span></span> <span data-ttu-id="aae1d-116">Ezt az előfeltételt csak engedélyezi a szolgáltatást, nem a tényleges felhasználói bejelentkezések esetén alkalmazható.</span><span class="sxs-lookup"><span data-stu-id="aae1d-116">This prerequisite is applicable only when you enable the feature, not for actual user sign-ins.</span></span>
- <span data-ttu-id="aae1d-117">Az Azure AD Connect közvetlen IP kapcsolatokat létesíthetik számára a [Azure adatközpont IP-címtartományok](https://www.microsoft.com/download/details.aspx?id=41653).</span><span class="sxs-lookup"><span data-stu-id="aae1d-117">Azure AD Connect can make direct IP connections to the [Azure data center IP ranges](https://www.microsoft.com/download/details.aspx?id=41653).</span></span> <span data-ttu-id="aae1d-118">Ebben az esetben a előfeltétel csak akkor, ha a funkció engedélyezéséhez, alkalmazható.</span><span class="sxs-lookup"><span data-stu-id="aae1d-118">Again, this prerequisite is applicable only when you enable the feature.</span></span>
2. <span data-ttu-id="aae1d-119">Minden AD-erdőben, amely szinkronizálja az Azure AD (Azure AD Connect használatával), és amelynek a felhasználók számára zökkenőmentes SSO engedélyezni szeretné tartományi rendszergazdai hitelesítő adatokra van szükség.</span><span class="sxs-lookup"><span data-stu-id="aae1d-119">You need Domain Administrator credentials for each AD forest that you synchronize to Azure AD (using Azure AD Connect) and for whose users you want to enable Seamless SSO.</span></span>

## <a name="step-2-enable-the-feature"></a><span data-ttu-id="aae1d-120">2. lépés: A funkció engedélyezése</span><span class="sxs-lookup"><span data-stu-id="aae1d-120">Step 2: Enable the feature</span></span>

<span data-ttu-id="aae1d-121">Zökkenőmentes SSO használatával engedélyezhető [az Azure AD Connect](active-directory-aadconnect.md).</span><span class="sxs-lookup"><span data-stu-id="aae1d-121">Seamless SSO can be enabled using [Azure AD Connect](active-directory-aadconnect.md).</span></span>

<span data-ttu-id="aae1d-122">Ha az Azure AD Connect új példánya, válassza ki a [egyéni telepítési útvonal](active-directory-aadconnect-get-started-custom.md).</span><span class="sxs-lookup"><span data-stu-id="aae1d-122">If you are doing a fresh installation of Azure AD Connect, choose the [custom installation path](active-directory-aadconnect-get-started-custom.md).</span></span> <span data-ttu-id="aae1d-123">A "Felhasználói bejelentkezés" lapon jelölje be a "Engedélyezése egyszeri bejelentkezéshez" beállítást.</span><span class="sxs-lookup"><span data-stu-id="aae1d-123">At the "User sign-in" page, check the "Enable single sign on" option.</span></span>

![Az Azure AD Connect - felhasználói bejelentkezés](./media/active-directory-aadconnect-sso/sso8.png)

<span data-ttu-id="aae1d-125">Ha már rendelkezik egy Azure AD Connect telepítése, válassza a "Módosítási felhasználói bejelentkezési oldalon" az Azure AD Connect, és kattintson a "Tovább gombra".</span><span class="sxs-lookup"><span data-stu-id="aae1d-125">If you already have an installation of Azure AD Connect, choose "Change user sign-in page" on Azure AD Connect and click "Next".</span></span> <span data-ttu-id="aae1d-126">Ezután jelölje be a "Engedélyezése egyszeri bejelentkezéshez" beállítást.</span><span class="sxs-lookup"><span data-stu-id="aae1d-126">Then check the "Enable single sign on" option.</span></span>

![Az Azure AD Connect - módosítás felhasználói bejelentkezés](./media/active-directory-aadconnect-user-signin/changeusersignin.png)

<span data-ttu-id="aae1d-128">Kövesse a varázsló utasításait, amíg a "Engedélyezése egyszeri bejelentkezéshez" a lap.</span><span class="sxs-lookup"><span data-stu-id="aae1d-128">Continue through the wizard until you get to the "Enable single sign on" page.</span></span> <span data-ttu-id="aae1d-129">Tartományi rendszergazda hitelesítő adatai minden AD-erdőben, amely szinkronizálja az Azure AD (Azure AD Connect használatával), és amelynek a felhasználók számára zökkenőmentes SSO engedélyezni szeretné.</span><span class="sxs-lookup"><span data-stu-id="aae1d-129">Provide Domain Administrator credentials for each AD forest that you synchronize to Azure AD (using Azure AD Connect) and for whose users you want to enable Seamless SSO.</span></span> 

<span data-ttu-id="aae1d-130">A varázsló befejezése után a zökkenőmentes SSO engedélyezve van a tenant.</span><span class="sxs-lookup"><span data-stu-id="aae1d-130">After completion of the wizard, Seamless SSO is enabled on your tenant.</span></span>

>[!NOTE]
> <span data-ttu-id="aae1d-131">A tartományi rendszergazda hitelesítő adatai nem tárolja az Azure AD Connectben vagy az Azure ad-ben, de a funkció engedélyezéséhez csak használható.</span><span class="sxs-lookup"><span data-stu-id="aae1d-131">The Domain Administrator credentials are not stored in Azure AD Connect or in Azure AD, but are only used to enable the feature.</span></span>

<span data-ttu-id="aae1d-132">Következő lépések követésével ellenőrizze, hogy engedélyezte zökkenőmentes SSO megfelelően:</span><span class="sxs-lookup"><span data-stu-id="aae1d-132">Follow these instructions to verify that you have enabled Seamless SSO correctly:</span></span>

1. <span data-ttu-id="aae1d-133">Jelentkezzen be a [Azure Active Directory felügyeleti központ](https://aad.portal.azure.com) a bérlő számára a globális rendszergazdai hitelesítő adataival.</span><span class="sxs-lookup"><span data-stu-id="aae1d-133">Sign in to the [Azure Active Directory admin center](https://aad.portal.azure.com) with the Global Administrator credentials for your tenant.</span></span>
2. <span data-ttu-id="aae1d-134">Válassza ki **Azure Active Directory** a bal oldali navigációs.</span><span class="sxs-lookup"><span data-stu-id="aae1d-134">Select **Azure Active Directory** on the left-hand navigation.</span></span>
3. <span data-ttu-id="aae1d-135">Válassza ki **az Azure AD Connect**.</span><span class="sxs-lookup"><span data-stu-id="aae1d-135">Select **Azure AD Connect**.</span></span>
4. <span data-ttu-id="aae1d-136">Ellenőrizze, hogy a **zökkenőmentes egyszeri bejelentkezést** funkció jelenít meg, mint a **engedélyezve**.</span><span class="sxs-lookup"><span data-stu-id="aae1d-136">Verify that the **Seamless Single Sign-On** feature shows as **Enabled**.</span></span>

![Azure portál – az Azure AD Connect panel](./media/active-directory-aadconnect-sso/sso10.png)

## <a name="step-3-roll-out-the-feature"></a><span data-ttu-id="aae1d-138">3. lépés: A szolgáltatás megkezdik</span><span class="sxs-lookup"><span data-stu-id="aae1d-138">Step 3: Roll out the feature</span></span>

<span data-ttu-id="aae1d-139">Számára, hogy a szolgáltatás a felhasználók számára, szeretné hozzáadni a két Azure AD URL-címet (https://autologon.microsoftazuread-sso.com és https://aadg.windows.net.nsatc.net) felhasználók Intranet zóna beállításainak az Active Directory csoportházirend segítségével.</span><span class="sxs-lookup"><span data-stu-id="aae1d-139">To roll out the feature to your users, you need to add two Azure AD URLs (https://autologon.microsoftazuread-sso.com and https://aadg.windows.net.nsatc.net) to users' Intranet zone settings via Group Policy in Active Directory.</span></span>

>[!NOTE]
> <span data-ttu-id="aae1d-140">Az alábbi utasítások csak működnek az Internet Explorer és a Google Chrome Windows (ha az Internet Explorer Megbízható helyek URL-osztanak azt).</span><span class="sxs-lookup"><span data-stu-id="aae1d-140">The following instructions only work for Internet Explorer and Google Chrome on Windows  (if it shares set of trusted site URLs with Internet Explorer).</span></span> <span data-ttu-id="aae1d-141">Olvassa el a következő részben Mozilla Firefox és a Google Chrome a Mac beállítása</span><span class="sxs-lookup"><span data-stu-id="aae1d-141">Read the next section for instructions to set up Mozilla Firefox and Google Chrome on Mac.</span></span>

### <a name="why-do-you-need-to-modify-users-intranet-zone-settings"></a><span data-ttu-id="aae1d-142">Miért van szüksége a felhasználói intranetes beállítások módosítása?</span><span class="sxs-lookup"><span data-stu-id="aae1d-142">Why do you need to modify users' Intranet zone settings?</span></span>

<span data-ttu-id="aae1d-143">Alapértelmezés szerint a böngésző automatikusan számítja ki a megfelelő zónához (internetes vagy intranetes) URL-címről.</span><span class="sxs-lookup"><span data-stu-id="aae1d-143">By default, the browser automatically calculates the right zone (Internet or Intranet) from a URL.</span></span> <span data-ttu-id="aae1d-144">Például http://contoso/ van leképezve az Intranet zónához, mivel http://intranet.contoso.com/ van rendelve az Internet zóna (mert az URL-cím pontot tartalmaz).</span><span class="sxs-lookup"><span data-stu-id="aae1d-144">For example, http://contoso/ is mapped to the Intranet zone, whereas http://intranet.contoso.com/ is mapped to the Internet zone (because the URL contains a period).</span></span> <span data-ttu-id="aae1d-145">Böngészők ne küldjön Kerberos-jegyek küldenek a felhővégpontnak – például a két Azure AD URL-címek – kivéve, ha az URL-cím kifejezetten a böngésző Intranet zónához.</span><span class="sxs-lookup"><span data-stu-id="aae1d-145">Browsers don't send Kerberos tickets to a cloud endpoint - like the two Azure AD URLs - unless its URL is explicitly added to the browser's Intranet zone.</span></span>

### <a name="detailed-steps"></a><span data-ttu-id="aae1d-146">Részletes lépések</span><span class="sxs-lookup"><span data-stu-id="aae1d-146">Detailed steps</span></span>

1. <span data-ttu-id="aae1d-147">Nyissa meg a Csoportházirend kezelése eszközt.</span><span class="sxs-lookup"><span data-stu-id="aae1d-147">Open the Group Policy Management tool.</span></span>
2. <span data-ttu-id="aae1d-148">Az egyes alkalmazott csoportházirend szerkesztése vagy a felhasználók.</span><span class="sxs-lookup"><span data-stu-id="aae1d-148">Edit the Group Policy that is applied to some or all your users.</span></span> <span data-ttu-id="aae1d-149">A jelen példában használjuk a **alapértelmezett tartományházirend**.</span><span class="sxs-lookup"><span data-stu-id="aae1d-149">In this example, we use the **Default Domain Policy**.</span></span>
3. <span data-ttu-id="aae1d-150">Navigáljon a **Felhasználó konfigurációja\Felügyeleti sablonok\Windows összetevők\Internet Explorer\Internet vezérlő Panel\Security lap** válassza **zónákhoz való társításának listája a hely**.</span><span class="sxs-lookup"><span data-stu-id="aae1d-150">Navigate to **User Configuration\Administrative Templates\Windows Components\Internet Explorer\Internet Control Panel\Security Page** and select **Site to Zone Assignment List**.</span></span>
<span data-ttu-id="aae1d-151">![Egyszeri bejelentkezés](./media/active-directory-aadconnect-sso/sso6.png)</span><span class="sxs-lookup"><span data-stu-id="aae1d-151">![Single sign-on](./media/active-directory-aadconnect-sso/sso6.png)</span></span>  
4. <span data-ttu-id="aae1d-152">Engedélyezi a házirendet, és adja meg a következő értékeket (az Azure AD URL-címek amelyben továbbított Kerberos-jegyek) és adatok (*1* Intranet zóna jelzi) párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="aae1d-152">Enable the policy, and enter the following values (Azure AD URLs where Kerberos tickets are forwarded) and data (*1* indicates Intranet zone) in the dialog box.</span></span>

        Value: https://autologon.microsoftazuread-sso.com
        Data: 1
        Value: https://aadg.windows.net.nsatc.net
        Data: 1
>[!NOTE]
> <span data-ttu-id="aae1d-153">Ha azt szeretné, ha ezek a felhasználók a megosztott számítógépeken - bejelentkezés nem engedélyezi a zökkenőmentes SSO - példányhoz, egyes felhasználók állítsa be a fenti értékeket *4*.</span><span class="sxs-lookup"><span data-stu-id="aae1d-153">If you want to disallow some users from using Seamless SSO - for instance, if these users are signing in on shared kiosks - set the preceding values to *4*.</span></span> <span data-ttu-id="aae1d-154">Ez a művelet az Azure AD URL-címeket ad hozzá a Tiltott helyek zóna, és zökkenőmentes SSO folyamatosan meghiúsul.</span><span class="sxs-lookup"><span data-stu-id="aae1d-154">This action adds the Azure AD URLs to the Restricted Zone, and fails Seamless SSO all the time.</span></span>

5. <span data-ttu-id="aae1d-155">Kattintson a **OK** és **OK** újra.</span><span class="sxs-lookup"><span data-stu-id="aae1d-155">Click **OK** and **OK** again.</span></span>

![Egyszeri bejelentkezés](./media/active-directory-aadconnect-sso/sso7.png)

### <a name="browser-considerations"></a><span data-ttu-id="aae1d-157">Böngésző kapcsolatos szempontok</span><span class="sxs-lookup"><span data-stu-id="aae1d-157">Browser considerations</span></span>

#### <a name="mozilla-firefox"></a><span data-ttu-id="aae1d-158">Mozilla Firefox</span><span class="sxs-lookup"><span data-stu-id="aae1d-158">Mozilla Firefox</span></span>

<span data-ttu-id="aae1d-159">Mozilla Firefox automatikusan nem használ Kerberos-hitelesítést.</span><span class="sxs-lookup"><span data-stu-id="aae1d-159">Mozilla Firefox doesn't automatically do Kerberos Authentication.</span></span> <span data-ttu-id="aae1d-160">Minden felhasználó rendelkezik-e manuálisan az Azure AD URL-címek hozzáadása a Firefox beállításait, az alábbi lépéseket követve:</span><span class="sxs-lookup"><span data-stu-id="aae1d-160">Each user has to manually add the Azure AD URLs to their Firefox settings using the following steps:</span></span>
1. <span data-ttu-id="aae1d-161">Futtassa a Firefox, és adja meg `about:config` a böngésző címsorába.</span><span class="sxs-lookup"><span data-stu-id="aae1d-161">Run Firefox and enter `about:config` in the address bar.</span></span> <span data-ttu-id="aae1d-162">Hagyja figyelmen kívül belőle értesítéseket, amelyek akkor jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="aae1d-162">Dismiss any notifications that you see.</span></span>
2. <span data-ttu-id="aae1d-163">Keresse meg a **network.negotiate-auth.trusted-URI-azonosítók** beállítás.</span><span class="sxs-lookup"><span data-stu-id="aae1d-163">Search for the **network.negotiate-auth.trusted-uris** preference.</span></span> <span data-ttu-id="aae1d-164">Ez a beállítás a Firefox a megbízható helyek a Kerberos-hitelesítést sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="aae1d-164">This preference lists Firefox's trusted sites for Kerberos authentication.</span></span>
3. <span data-ttu-id="aae1d-165">Kattintson a jobb gombbal, és válassza a "Módosítás".</span><span class="sxs-lookup"><span data-stu-id="aae1d-165">Right-click and select "Modify".</span></span>
4. <span data-ttu-id="aae1d-166">Adja meg "https://autologon.microsoftazuread-sso.com, https://aadg.windows.net.nsatc.net" mezőjében.</span><span class="sxs-lookup"><span data-stu-id="aae1d-166">Enter "https://autologon.microsoftazuread-sso.com, https://aadg.windows.net.nsatc.net" in the field.</span></span>
5. <span data-ttu-id="aae1d-167">Az "OK" gombra, majd nyissa meg a böngészőben.</span><span class="sxs-lookup"><span data-stu-id="aae1d-167">Click "OK" and reopen the browser.</span></span>

#### <a name="safari-on-mac-os"></a><span data-ttu-id="aae1d-168">Mac OS Safari</span><span class="sxs-lookup"><span data-stu-id="aae1d-168">Safari on Mac OS</span></span>

<span data-ttu-id="aae1d-169">Győződjön meg arról, hogy a Mac OS futtató gép AD egy tartományhoz.</span><span class="sxs-lookup"><span data-stu-id="aae1d-169">Ensure that the machine running Mac OS is joined to AD.</span></span> <span data-ttu-id="aae1d-170">Kövesse az utasításokat, amelyek erre [Itt](http://training.apple.com/pdf/Best_Practices_for_Integrating_OS_X_with_Active_Directory.pdf).</span><span class="sxs-lookup"><span data-stu-id="aae1d-170">See instructions on how to do that [here](http://training.apple.com/pdf/Best_Practices_for_Integrating_OS_X_with_Active_Directory.pdf).</span></span>

#### <a name="google-chrome-on-mac-os"></a><span data-ttu-id="aae1d-171">Google Chrome Mac OS</span><span class="sxs-lookup"><span data-stu-id="aae1d-171">Google Chrome on Mac OS</span></span>

<span data-ttu-id="aae1d-172">Google Chrome a Mac OS és más nem Windows platformokon, tekintse meg a [Ez a cikk](https://dev.chromium.org/administrators/policy-list-3#AuthServerWhitelist) integrált hitelesítés engedélyezett az Azure AD URL-címek olvashat.</span><span class="sxs-lookup"><span data-stu-id="aae1d-172">For Google Chrome on Mac OS and other non-Windows platforms, refer to [this article](https://dev.chromium.org/administrators/policy-list-3#AuthServerWhitelist) for information on how to whitelist the Azure AD URLs for integrated authentication.</span></span>

<span data-ttu-id="aae1d-173">Segítségével a külső Active Directory csoportházirend-bővítmények számára, hogy az Azure AD URL-címek Firefox és Google Chrome Mac-felhasználók nem érhető el ez a cikk.</span><span class="sxs-lookup"><span data-stu-id="aae1d-173">Using third-party Active Directory Group Policy extensions to roll out the Azure AD URLs to Firefox and Google Chrome on Mac users is outside of this article's scope.</span></span>

#### <a name="known-limitations"></a><span data-ttu-id="aae1d-174">Ismert korlátozásai</span><span class="sxs-lookup"><span data-stu-id="aae1d-174">Known limitations</span></span>

<span data-ttu-id="aae1d-175">Privát böngészés módban a Firefox és Edge böngésző zökkenőmentes SSO nem működik.</span><span class="sxs-lookup"><span data-stu-id="aae1d-175">Seamless SSO doesn't work in private browsing mode on Firefox and Edge browsers.</span></span> <span data-ttu-id="aae1d-176">Is nem működik az Internet Explorer böngészőben fokozott védelmet módban fut. Ha.</span><span class="sxs-lookup"><span data-stu-id="aae1d-176">It also doesn't work on Internet Explorer if the browser is running in Enhanced Protection mode.</span></span>

>[!IMPORTANT]
><span data-ttu-id="aae1d-177">A Microsoft nemrég visszaállítása vizsgálja meg az ügyfél által jelentett problémák peremhálózati támogatása.</span><span class="sxs-lookup"><span data-stu-id="aae1d-177">We recently rolled back support for Edge to investigate customer-reported issues.</span></span>

## <a name="step-4-test-the-feature"></a><span data-ttu-id="aae1d-178">4. lépés: A szolgáltatás tesztelése</span><span class="sxs-lookup"><span data-stu-id="aae1d-178">Step 4: Test the feature</span></span>

<span data-ttu-id="aae1d-179">A szolgáltatás adott felhasználó teszteléséhez győződjön meg arról, hogy _összes_ helyen vannak a következő feltételeknek:</span><span class="sxs-lookup"><span data-stu-id="aae1d-179">To test the feature for a specific user, ensure that _all_ the following conditions are in place:</span></span>
  - <span data-ttu-id="aae1d-180">A felhasználó egy vállalati eszközre jelentkezik be.</span><span class="sxs-lookup"><span data-stu-id="aae1d-180">The user is signing in on a corporate device.</span></span>
  - <span data-ttu-id="aae1d-181">Az eszköz korábban csatlakoztatva lett az Active Directory (AD) tartományhoz.</span><span class="sxs-lookup"><span data-stu-id="aae1d-181">The device has been previously joined to your Active Directory (AD) domain.</span></span>
  - <span data-ttu-id="aae1d-182">Az eszköz számára a tartományvezérlőn (DC), vagy a vállalati vezetékes vagy vezeték nélküli hálózat vagy távelérési kapcsolatot, például a VPN-kapcsolaton keresztül közvetlen kapcsolattal rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="aae1d-182">The device has a direct connection to your Domain Controller (DC), either on the corporate wired or wireless network or via a remote access connection, such as a VPN connection.</span></span>
  - <span data-ttu-id="aae1d-183">Rendelkezik [megkezdődött a szolgáltatás](##step-3-roll-out-the-feature) csoportházirend használatával a felhasználóhoz.</span><span class="sxs-lookup"><span data-stu-id="aae1d-183">You have [rolled out the feature](##step-3-roll-out-the-feature) to this user using Group Policy.</span></span>

<span data-ttu-id="aae1d-184">A forgatókönyv, ahol a felhasználó beírja, csak a felhasználónév, de nem a jelszó ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="aae1d-184">To test the scenario where the user enters only the username, but not the password:</span></span>
- <span data-ttu-id="aae1d-185">Jelentkezzen be a *https://myapps.microsoft.com/* egy új titkos böngésző-munkamenetben.</span><span class="sxs-lookup"><span data-stu-id="aae1d-185">Sign into *https://myapps.microsoft.com/* in a new private browser session.</span></span>

<span data-ttu-id="aae1d-186">A tesztelje a forgatókönyvet, ahol a felhasználó nem rendelkezik a felhasználónév vagy jelszó megadását:</span><span class="sxs-lookup"><span data-stu-id="aae1d-186">To test the scenario where the user doesn't have to enter the username or the password:</span></span> 
- <span data-ttu-id="aae1d-187">Jelentkezzen be a *https://myapps.microsoft.com/contoso.onmicrosoft.com* egy új titkos böngésző-munkamenetben.</span><span class="sxs-lookup"><span data-stu-id="aae1d-187">Sign into *https://myapps.microsoft.com/contoso.onmicrosoft.com* in a new private browser session.</span></span> <span data-ttu-id="aae1d-188">Cserélje le "*contoso*" a bérlő névvel.</span><span class="sxs-lookup"><span data-stu-id="aae1d-188">Replace "*contoso*" with your tenant's name.</span></span>
- <span data-ttu-id="aae1d-189">Jelentkezzen be vagy *https://myapps.microsoft.com/contoso.com* egy új titkos böngésző-munkamenetben.</span><span class="sxs-lookup"><span data-stu-id="aae1d-189">Or sign into *https://myapps.microsoft.com/contoso.com* in a new private browser session.</span></span> <span data-ttu-id="aae1d-190">Cserélje le "*contoso.com*" a bérlő ellenőrzött tartomány (nem összevont tartományhoz).</span><span class="sxs-lookup"><span data-stu-id="aae1d-190">Replace "*contoso.com*" with a verified domain (not a federated domain) in your tenant.</span></span>

## <a name="step-5-roll-over-keys"></a><span data-ttu-id="aae1d-191">5. lépés: A kulcs váltása</span><span class="sxs-lookup"><span data-stu-id="aae1d-191">Step 5: Roll over keys</span></span>

<span data-ttu-id="aae1d-192">2. lépésben az Azure AD Connect számítógépfiókot hoz létre (az Azure AD közti) amelyiken engedélyezte a zökkenőmentes SSO minden AD-erdőben.</span><span class="sxs-lookup"><span data-stu-id="aae1d-192">In Step 2, Azure AD Connect creates computer accounts (representing Azure AD) in all the AD forests on which you have enabled Seamless SSO.</span></span> <span data-ttu-id="aae1d-193">Ismerje meg, a részletesebb [Itt](active-directory-aadconnect-sso-how-it-works.md).</span><span class="sxs-lookup"><span data-stu-id="aae1d-193">Learn more in detail [here](active-directory-aadconnect-sso-how-it-works.md).</span></span> <span data-ttu-id="aae1d-194">A nagyobb biztonság érdekében javasoljuk, hogy Ön gyakran váltása a Kerberos visszafejtési kulcs esetében a számítógépfiókok.</span><span class="sxs-lookup"><span data-stu-id="aae1d-194">For improved security, it is recommended that  you frequently roll over the Kerberos decryption keys of these computer accounts.</span></span>

>[!IMPORTANT]
><span data-ttu-id="aae1d-195">Végezze el ezt a lépést nem kell _azonnal_ a szolgáltatás engedélyezése után.</span><span class="sxs-lookup"><span data-stu-id="aae1d-195">You don't need to do this step _immediately_ after you have enabled the feature.</span></span> <span data-ttu-id="aae1d-196">Váltása a Kerberos-visszafejtési kulcsok legalább 30 nap.</span><span class="sxs-lookup"><span data-stu-id="aae1d-196">Roll over the Kerberos decryption keys at least every 30 days.</span></span>

## <a name="next-steps"></a><span data-ttu-id="aae1d-197">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="aae1d-197">Next steps</span></span>

- <span data-ttu-id="aae1d-198">[**Műszaki mélyreható** ](active-directory-aadconnect-sso-how-it-works.md) – Ez a funkció működésének megismerése.</span><span class="sxs-lookup"><span data-stu-id="aae1d-198">[**Technical Deep Dive**](active-directory-aadconnect-sso-how-it-works.md) - Understand how this feature works.</span></span>
- <span data-ttu-id="aae1d-199">[**Gyakran ismételt kérdések** ](active-directory-aadconnect-sso-faq.md) -gyakran feltett kérdésekre adott válaszokat.</span><span class="sxs-lookup"><span data-stu-id="aae1d-199">[**Frequently Asked Questions**](active-directory-aadconnect-sso-faq.md) - Answers to frequently asked questions.</span></span>
- <span data-ttu-id="aae1d-200">[**Hibaelhárítás** ](active-directory-aadconnect-troubleshoot-sso.md) -Útmutató: a szolgáltatással kapcsolatos gyakori problémák megoldása.</span><span class="sxs-lookup"><span data-stu-id="aae1d-200">[**Troubleshoot**](active-directory-aadconnect-troubleshoot-sso.md) - Learn how to resolve common issues with the feature.</span></span>
- <span data-ttu-id="aae1d-201">[**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) – új funkciókérések tárolásához.</span><span class="sxs-lookup"><span data-stu-id="aae1d-201">[**UserVoice**](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) - For filing new feature requests.</span></span>
