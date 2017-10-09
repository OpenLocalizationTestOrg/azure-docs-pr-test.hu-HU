---
title: "Az Azure AD Connect: Zökkenőmentes egyszeri bejelentkezés – első lépések |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan tooget el az Azure Active Directory zökkenőmentes egyszeri bejelentkezést."
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
ms.openlocfilehash: 97d40ed41b3bfad9ff7e11ddbdb8de594ee85de1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-seamless-single-sign-on-quick-start"></a><span data-ttu-id="583b2-104">Az Azure Active Directory zökkenőmentes egyszeri bejelentkezés: első lépések</span><span class="sxs-lookup"><span data-stu-id="583b2-104">Azure Active Directory Seamless Single Sign-On: Quick start</span></span>

## <a name="how-toodeploy-seamless-sso"></a><span data-ttu-id="583b2-105">Hogyan toodeploy zökkenőmentes SSO</span><span class="sxs-lookup"><span data-stu-id="583b2-105">How toodeploy Seamless SSO</span></span>

<span data-ttu-id="583b2-106">Azure Active Directory zökkenőmentes egyszeri bejelentkezést (az Azure AD zökkenőmentes SSO) automatikusan amikor azok a vállalati asztali számítógép csatlakoztatott tooyour vállalati hálózaton lévő felhasználók jelentkezik.</span><span class="sxs-lookup"><span data-stu-id="583b2-106">Azure Active Directory Seamless Single Sign-On (Azure AD Seamless SSO) automatically signs users in when they are on their corporate desktops connected tooyour corporate network.</span></span> <span data-ttu-id="583b2-107">Hozzáférést biztosít a a felhasználók könnyen tooyour felhőalapú alkalmazások anélkül, hogy semmilyen további helyszíni összetevőt.</span><span class="sxs-lookup"><span data-stu-id="583b2-107">It provides your users easy access tooyour cloud-based applications without needing any additional on-premises components.</span></span>

>[!IMPORTANT]
><span data-ttu-id="583b2-108">hello zökkenőmentes SSO funkció jelenleg előzetes verzió.</span><span class="sxs-lookup"><span data-stu-id="583b2-108">hello Seamless SSO feature is currently in preview.</span></span>

<span data-ttu-id="583b2-109">toofollow szüksége toodeploy zökkenőmentes SSO, ezeket a lépéseket:</span><span class="sxs-lookup"><span data-stu-id="583b2-109">toodeploy Seamless SSO, you need toofollow these steps:</span></span>

## <a name="step-1-check-prerequisites"></a><span data-ttu-id="583b2-110">1. lépés: Követelmények ellenőrzésének megismétlése</span><span class="sxs-lookup"><span data-stu-id="583b2-110">Step 1: Check prerequisites</span></span>

<span data-ttu-id="583b2-111">Győződjön meg arról, hogy hello előfeltételeiről a következő helyen:</span><span class="sxs-lookup"><span data-stu-id="583b2-111">Ensure that hello following prerequisites are in place:</span></span>

1. <span data-ttu-id="583b2-112">Az Azure AD Connect-kiszolgáló beállítása: Ha [áteresztő hitelesítés](active-directory-aadconnect-pass-through-authentication.md) legyen a bejelentkezési módszer semmilyen további műveletet nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="583b2-112">Set up your Azure AD Connect server: If you use [Pass-through Authentication](active-directory-aadconnect-pass-through-authentication.md) as your sign-in method, no further action is required.</span></span> <span data-ttu-id="583b2-113">Ha [Jelszókivonat-szinkronizálást](active-directory-aadconnectsync-implement-password-synchronization.md) legyen a bejelentkezési módszer, és ha az Azure AD Connect és az Azure AD között tűzfal, ellenőrizze, hogy:</span><span class="sxs-lookup"><span data-stu-id="583b2-113">If you use [Password Hash Synchronization](active-directory-aadconnectsync-implement-password-synchronization.md) as your sign-in method, and if there is a firewall between Azure AD Connect and Azure AD, ensure that:</span></span>
- <span data-ttu-id="583b2-114">Verziók 1.1.484.0 használ vagy újabb, az Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="583b2-114">You are using versions 1.1.484.0 or later of Azure AD Connect.</span></span>
- <span data-ttu-id="583b2-115">Az Azure AD Connect kommunikálhatnak `*.msappproxy.net` URL-címek és a 443-as porton alatt.</span><span class="sxs-lookup"><span data-stu-id="583b2-115">Azure AD Connect can communicate with `*.msappproxy.net` URLs and over port 443.</span></span> <span data-ttu-id="583b2-116">Ezt az előfeltételt csak engedélyezi hello szolgáltatást, nem a tényleges felhasználói bejelentkezések esetén alkalmazható.</span><span class="sxs-lookup"><span data-stu-id="583b2-116">This prerequisite is applicable only when you enable hello feature, not for actual user sign-ins.</span></span>
- <span data-ttu-id="583b2-117">Az Azure AD Connect tehet közvetlen IP-kapcsolatok toohello [Azure adatközpont IP-címtartományok](https://www.microsoft.com/download/details.aspx?id=41653).</span><span class="sxs-lookup"><span data-stu-id="583b2-117">Azure AD Connect can make direct IP connections toohello [Azure data center IP ranges](https://www.microsoft.com/download/details.aspx?id=41653).</span></span> <span data-ttu-id="583b2-118">Ebben az esetben ezt az előfeltételt alkalmazható csak akkor engedélyezi hello szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="583b2-118">Again, this prerequisite is applicable only when you enable hello feature.</span></span>
2. <span data-ttu-id="583b2-119">Tartományi rendszergazdai hitelesítő adatokra van szüksége minden tooAzure AD szinkronizálja Active Directory-erdőben (az Azure AD Connect használatával) és amelynek felhasználók tooenable zökkenőmentes SSO keresi.</span><span class="sxs-lookup"><span data-stu-id="583b2-119">You need Domain Administrator credentials for each AD forest that you synchronize tooAzure AD (using Azure AD Connect) and for whose users you want tooenable Seamless SSO.</span></span>

## <a name="step-2-enable-hello-feature"></a><span data-ttu-id="583b2-120">2. lépés: Hello szolgáltatás engedélyezése</span><span class="sxs-lookup"><span data-stu-id="583b2-120">Step 2: Enable hello feature</span></span>

<span data-ttu-id="583b2-121">Zökkenőmentes SSO használatával engedélyezhető [az Azure AD Connect](active-directory-aadconnect.md).</span><span class="sxs-lookup"><span data-stu-id="583b2-121">Seamless SSO can be enabled using [Azure AD Connect](active-directory-aadconnect.md).</span></span>

<span data-ttu-id="583b2-122">Ha az Azure AD Connect új példánya, válassza ki a hello [egyéni telepítési útvonal](active-directory-aadconnect-get-started-custom.md).</span><span class="sxs-lookup"><span data-stu-id="583b2-122">If you are doing a fresh installation of Azure AD Connect, choose hello [custom installation path](active-directory-aadconnect-get-started-custom.md).</span></span> <span data-ttu-id="583b2-123">Hello "Felhasználói bejelentkezés" lapon jelölje be hello "Engedélyezése egyszeri bejelentkezéshez" beállítást.</span><span class="sxs-lookup"><span data-stu-id="583b2-123">At hello "User sign-in" page, check hello "Enable single sign on" option.</span></span>

![Az Azure AD Connect - felhasználói bejelentkezés](./media/active-directory-aadconnect-sso/sso8.png)

<span data-ttu-id="583b2-125">Ha már rendelkezik egy Azure AD Connect telepítése, válassza a "Módosítási felhasználói bejelentkezési oldalon" az Azure AD Connect, és kattintson a "Tovább gombra".</span><span class="sxs-lookup"><span data-stu-id="583b2-125">If you already have an installation of Azure AD Connect, choose "Change user sign-in page" on Azure AD Connect and click "Next".</span></span> <span data-ttu-id="583b2-126">Ezután jelölje be hello "Engedélyezése egyszeri bejelentkezéshez" beállítást.</span><span class="sxs-lookup"><span data-stu-id="583b2-126">Then check hello "Enable single sign on" option.</span></span>

![Az Azure AD Connect - módosítás felhasználói bejelentkezés](./media/active-directory-aadconnect-user-signin/changeusersignin.png)

<span data-ttu-id="583b2-128">Továbbra is hello varázsló lépéseinek, amíg elér toohello "Engedélyezése egyszeri bejelentkezéshez" lapon.</span><span class="sxs-lookup"><span data-stu-id="583b2-128">Continue through hello wizard until you get toohello "Enable single sign on" page.</span></span> <span data-ttu-id="583b2-129">Tartományi rendszergazdai hitelesítő adatok megadása minden Active Directory-erdőben tooAzure AD szinkronizálja (Azure AD Connect használatával) és amelynek felhasználók tooenable zökkenőmentes SSO keresi.</span><span class="sxs-lookup"><span data-stu-id="583b2-129">Provide Domain Administrator credentials for each AD forest that you synchronize tooAzure AD (using Azure AD Connect) and for whose users you want tooenable Seamless SSO.</span></span> 

<span data-ttu-id="583b2-130">Zökkenőmentes SSO hello varázsló befejezése után a bérlői engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="583b2-130">After completion of hello wizard, Seamless SSO is enabled on your tenant.</span></span>

>[!NOTE]
> <span data-ttu-id="583b2-131">hello tartományi rendszergazda hitelesítő adatai nem tárolja az Azure AD Connectben vagy az Azure ad-ben, de csak a felhasznált tooenable hello szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="583b2-131">hello Domain Administrator credentials are not stored in Azure AD Connect or in Azure AD, but are only used tooenable hello feature.</span></span>

<span data-ttu-id="583b2-132">Hajtsa végre az ezen utasításokat tooverify engedélyezte zökkenőmentes SSO megfelelően:</span><span class="sxs-lookup"><span data-stu-id="583b2-132">Follow these instructions tooverify that you have enabled Seamless SSO correctly:</span></span>

1. <span data-ttu-id="583b2-133">Jelentkezzen be toohello [Azure Active Directory felügyeleti központ](https://aad.portal.azure.com) hello globális rendszergazdai hitelesítő adataival, a bérlő számára.</span><span class="sxs-lookup"><span data-stu-id="583b2-133">Sign in toohello [Azure Active Directory admin center](https://aad.portal.azure.com) with hello Global Administrator credentials for your tenant.</span></span>
2. <span data-ttu-id="583b2-134">Válassza ki **Azure Active Directory** hello bal oldali navigációs.</span><span class="sxs-lookup"><span data-stu-id="583b2-134">Select **Azure Active Directory** on hello left-hand navigation.</span></span>
3. <span data-ttu-id="583b2-135">Válassza ki **az Azure AD Connect**.</span><span class="sxs-lookup"><span data-stu-id="583b2-135">Select **Azure AD Connect**.</span></span>
4. <span data-ttu-id="583b2-136">Győződjön meg arról, hogy hello **zökkenőmentes egyszeri bejelentkezést** funkció jelenít meg, mint a **engedélyezve**.</span><span class="sxs-lookup"><span data-stu-id="583b2-136">Verify that hello **Seamless Single Sign-On** feature shows as **Enabled**.</span></span>

![Azure portál – az Azure AD Connect panel](./media/active-directory-aadconnect-sso/sso10.png)

## <a name="step-3-roll-out-hello-feature"></a><span data-ttu-id="583b2-138">3. lépés: Megkezdik hello szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="583b2-138">Step 3: Roll out hello feature</span></span>

<span data-ttu-id="583b2-139">hello szolgáltatás tooyour felhasználók tooroll, meg kell tooadd két Azure AD URL-címek (https://autologon.microsoftazuread-sso.com és https://aadg.windows.net.nsatc.net) toousers' Intranet zóna beállításait az Active Directory csoportházirend segítségével.</span><span class="sxs-lookup"><span data-stu-id="583b2-139">tooroll out hello feature tooyour users, you need tooadd two Azure AD URLs (https://autologon.microsoftazuread-sso.com and https://aadg.windows.net.nsatc.net) toousers' Intranet zone settings via Group Policy in Active Directory.</span></span>

>[!NOTE]
> <span data-ttu-id="583b2-140">hello a következő utasítások csak olyan feladaton végezhető az Internet Explorer és a Google Chrome Windows (ha az Internet Explorer Megbízható helyek URL-osztanak azt).</span><span class="sxs-lookup"><span data-stu-id="583b2-140">hello following instructions only work for Internet Explorer and Google Chrome on Windows  (if it shares set of trusted site URLs with Internet Explorer).</span></span> <span data-ttu-id="583b2-141">Olvassa el a következő szakaszban hello utasításokat tooset Mozilla Firefox és a Mac Google Chrome</span><span class="sxs-lookup"><span data-stu-id="583b2-141">Read hello next section for instructions tooset up Mozilla Firefox and Google Chrome on Mac.</span></span>

### <a name="why-do-you-need-toomodify-users-intranet-zone-settings"></a><span data-ttu-id="583b2-142">Miért kell toomodify felhasználók Intranet zóna beállítását?</span><span class="sxs-lookup"><span data-stu-id="583b2-142">Why do you need toomodify users' Intranet zone settings?</span></span>

<span data-ttu-id="583b2-143">Alapértelmezés szerint hello böngésző automatikusan kiszámítja hello jobb zóna (internetes vagy intranetes) URL-címről.</span><span class="sxs-lookup"><span data-stu-id="583b2-143">By default, hello browser automatically calculates hello right zone (Internet or Intranet) from a URL.</span></span> <span data-ttu-id="583b2-144">Például http://contoso/ csatlakoztatott toohello intranetzónához, mivel http://intranet.contoso.com/ csatlakoztatott toohello Internet zóna (mert hello URL-cím pontot tartalmaz).</span><span class="sxs-lookup"><span data-stu-id="583b2-144">For example, http://contoso/ is mapped toohello Intranet zone, whereas http://intranet.contoso.com/ is mapped toohello Internet zone (because hello URL contains a period).</span></span> <span data-ttu-id="583b2-145">Böngészők ne küldjön a Kerberos jegyek tooa felhővégpontnak - hello két Azure AD URL-címekhez hasonlít -, kivéve, ha az URL-cím van kifejezetten toohello böngésző Intranet zónához.</span><span class="sxs-lookup"><span data-stu-id="583b2-145">Browsers don't send Kerberos tickets tooa cloud endpoint - like hello two Azure AD URLs - unless its URL is explicitly added toohello browser's Intranet zone.</span></span>

### <a name="detailed-steps"></a><span data-ttu-id="583b2-146">Részletes lépések</span><span class="sxs-lookup"><span data-stu-id="583b2-146">Detailed steps</span></span>

1. <span data-ttu-id="583b2-147">Hello Csoportházirend kezelése eszköz megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="583b2-147">Open hello Group Policy Management tool.</span></span>
2. <span data-ttu-id="583b2-148">Hello csoportházirend által alkalmazott toosome vagy a felhasználók szerkesztése.</span><span class="sxs-lookup"><span data-stu-id="583b2-148">Edit hello Group Policy that is applied toosome or all your users.</span></span> <span data-ttu-id="583b2-149">A jelen példában használjuk hello **alapértelmezett tartományházirend**.</span><span class="sxs-lookup"><span data-stu-id="583b2-149">In this example, we use hello **Default Domain Policy**.</span></span>
3. <span data-ttu-id="583b2-150">Keresse meg a túl**Felhasználó konfigurációja\Felügyeleti sablonok\Windows összetevők\Internet Explorer\Internet vezérlő Panel\Security lap** válassza **tooZone társításának listája hely**.</span><span class="sxs-lookup"><span data-stu-id="583b2-150">Navigate too**User Configuration\Administrative Templates\Windows Components\Internet Explorer\Internet Control Panel\Security Page** and select **Site tooZone Assignment List**.</span></span>
<span data-ttu-id="583b2-151">![Egyszeri bejelentkezés](./media/active-directory-aadconnect-sso/sso6.png)</span><span class="sxs-lookup"><span data-stu-id="583b2-151">![Single sign-on](./media/active-directory-aadconnect-sso/sso6.png)</span></span>  
4. <span data-ttu-id="583b2-152">Hello házirend engedélyezése, és írja be a következő értékeket (az Azure AD URL-címek amelyben továbbított Kerberos-jegyek) hello és adatok (*1* Intranet zóna jelzi) hello párbeszédpanelen.</span><span class="sxs-lookup"><span data-stu-id="583b2-152">Enable hello policy, and enter hello following values (Azure AD URLs where Kerberos tickets are forwarded) and data (*1* indicates Intranet zone) in hello dialog box.</span></span>

        Value: https://autologon.microsoftazuread-sso.com
        Data: 1
        Value: https://aadg.windows.net.nsatc.net
        Data: 1
>[!NOTE]
> <span data-ttu-id="583b2-153">Toodisallow egyes felhasználók a zökkenőmentes SSO - példányhoz, ha ezek a felhasználók a megosztott számítógépeken - bejelentkezés állítsa az értékek túl megelőző hello*4*.</span><span class="sxs-lookup"><span data-stu-id="583b2-153">If you want toodisallow some users from using Seamless SSO - for instance, if these users are signing in on shared kiosks - set hello preceding values too*4*.</span></span> <span data-ttu-id="583b2-154">Ez a művelet hello Azure AD URL-címek toohello tiltott helyek zóna hozzáadása, és zökkenőmentes SSO mindig hello sikertelen.</span><span class="sxs-lookup"><span data-stu-id="583b2-154">This action adds hello Azure AD URLs toohello Restricted Zone, and fails Seamless SSO all hello time.</span></span>

5. <span data-ttu-id="583b2-155">Kattintson a **OK** és **OK** újra.</span><span class="sxs-lookup"><span data-stu-id="583b2-155">Click **OK** and **OK** again.</span></span>

![Egyszeri bejelentkezés](./media/active-directory-aadconnect-sso/sso7.png)

### <a name="browser-considerations"></a><span data-ttu-id="583b2-157">Böngésző kapcsolatos szempontok</span><span class="sxs-lookup"><span data-stu-id="583b2-157">Browser considerations</span></span>

#### <a name="mozilla-firefox"></a><span data-ttu-id="583b2-158">Mozilla Firefox</span><span class="sxs-lookup"><span data-stu-id="583b2-158">Mozilla Firefox</span></span>

<span data-ttu-id="583b2-159">Mozilla Firefox automatikusan nem használ Kerberos-hitelesítést.</span><span class="sxs-lookup"><span data-stu-id="583b2-159">Mozilla Firefox doesn't automatically do Kerberos Authentication.</span></span> <span data-ttu-id="583b2-160">Minden felhasználó rendelkezik-e toomanually hello Azure AD URL-címek tootheir Firefox a beállításokat a következő lépéseket hello hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="583b2-160">Each user has toomanually add hello Azure AD URLs tootheir Firefox settings using hello following steps:</span></span>
1. <span data-ttu-id="583b2-161">Futtassa a Firefox, és adja meg `about:config` hello címsorába.</span><span class="sxs-lookup"><span data-stu-id="583b2-161">Run Firefox and enter `about:config` in hello address bar.</span></span> <span data-ttu-id="583b2-162">Hagyja figyelmen kívül belőle értesítéseket, amelyek akkor jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="583b2-162">Dismiss any notifications that you see.</span></span>
2. <span data-ttu-id="583b2-163">Keresse meg a hello **network.negotiate-auth.trusted-URI-azonosítók** beállítás.</span><span class="sxs-lookup"><span data-stu-id="583b2-163">Search for hello **network.negotiate-auth.trusted-uris** preference.</span></span> <span data-ttu-id="583b2-164">Ez a beállítás a Firefox a megbízható helyek a Kerberos-hitelesítést sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="583b2-164">This preference lists Firefox's trusted sites for Kerberos authentication.</span></span>
3. <span data-ttu-id="583b2-165">Kattintson a jobb gombbal, és válassza a "Módosítás".</span><span class="sxs-lookup"><span data-stu-id="583b2-165">Right-click and select "Modify".</span></span>
4. <span data-ttu-id="583b2-166">Adja meg "https://autologon.microsoftazuread-sso.com, https://aadg.windows.net.nsatc.net" hello mezőben.</span><span class="sxs-lookup"><span data-stu-id="583b2-166">Enter "https://autologon.microsoftazuread-sso.com, https://aadg.windows.net.nsatc.net" in hello field.</span></span>
5. <span data-ttu-id="583b2-167">Az "OK" gombra, majd nyissa meg a hello böngésző.</span><span class="sxs-lookup"><span data-stu-id="583b2-167">Click "OK" and reopen hello browser.</span></span>

#### <a name="safari-on-mac-os"></a><span data-ttu-id="583b2-168">Mac OS Safari</span><span class="sxs-lookup"><span data-stu-id="583b2-168">Safari on Mac OS</span></span>

<span data-ttu-id="583b2-169">Győződjön meg arról, hogy fut a Mac OS hello gép illesztett tooAD.</span><span class="sxs-lookup"><span data-stu-id="583b2-169">Ensure that hello machine running Mac OS is joined tooAD.</span></span> <span data-ttu-id="583b2-170">Útmutatás meg, hogyan toodo, amely [Itt](http://training.apple.com/pdf/Best_Practices_for_Integrating_OS_X_with_Active_Directory.pdf).</span><span class="sxs-lookup"><span data-stu-id="583b2-170">See instructions on how toodo that [here](http://training.apple.com/pdf/Best_Practices_for_Integrating_OS_X_with_Active_Directory.pdf).</span></span>

#### <a name="google-chrome-on-mac-os"></a><span data-ttu-id="583b2-171">Google Chrome Mac OS</span><span class="sxs-lookup"><span data-stu-id="583b2-171">Google Chrome on Mac OS</span></span>

<span data-ttu-id="583b2-172">Google Chrome a Mac OS és más nem Windows platformokon, tekintse meg túl[Ez a cikk](https://dev.chromium.org/administrators/policy-list-3#AuthServerWhitelist) hogyan toowhitelist hello integrált hitelesítés URL-címeit az Azure AD tájékoztatást.</span><span class="sxs-lookup"><span data-stu-id="583b2-172">For Google Chrome on Mac OS and other non-Windows platforms, refer too[this article](https://dev.chromium.org/administrators/policy-list-3#AuthServerWhitelist) for information on how toowhitelist hello Azure AD URLs for integrated authentication.</span></span>

<span data-ttu-id="583b2-173">Külső Active Directory csoportházirend segítségével bővítmények tooroll hello Azure AD URL-címek tooFirefox és Google Chrome Mac-felhasználók nem érhető el ez a cikk.</span><span class="sxs-lookup"><span data-stu-id="583b2-173">Using third-party Active Directory Group Policy extensions tooroll out hello Azure AD URLs tooFirefox and Google Chrome on Mac users is outside of this article's scope.</span></span>

#### <a name="known-limitations"></a><span data-ttu-id="583b2-174">Ismert korlátozásai</span><span class="sxs-lookup"><span data-stu-id="583b2-174">Known limitations</span></span>

<span data-ttu-id="583b2-175">Privát böngészés módban a Firefox és Edge böngésző zökkenőmentes SSO nem működik.</span><span class="sxs-lookup"><span data-stu-id="583b2-175">Seamless SSO doesn't work in private browsing mode on Firefox and Edge browsers.</span></span> <span data-ttu-id="583b2-176">Még nem működik az Internet Explorer hello böngésző fokozott védelmet módban fut. Ha.</span><span class="sxs-lookup"><span data-stu-id="583b2-176">It also doesn't work on Internet Explorer if hello browser is running in Enhanced Protection mode.</span></span>

>[!IMPORTANT]
><span data-ttu-id="583b2-177">A Microsoft nemrég visszaállítása peremhálózati tooinvestigate támogatása ügyfél jelentett problémák.</span><span class="sxs-lookup"><span data-stu-id="583b2-177">We recently rolled back support for Edge tooinvestigate customer-reported issues.</span></span>

## <a name="step-4-test-hello-feature"></a><span data-ttu-id="583b2-178">4. lépés: Hello szolgáltatás tesztelése</span><span class="sxs-lookup"><span data-stu-id="583b2-178">Step 4: Test hello feature</span></span>

<span data-ttu-id="583b2-179">tootest hello szolgáltatás adott felhasználó, ügyeljen arra, hogy _összes_ hello a következő feltételek érvényesek:</span><span class="sxs-lookup"><span data-stu-id="583b2-179">tootest hello feature for a specific user, ensure that _all_ hello following conditions are in place:</span></span>
  - <span data-ttu-id="583b2-180">Vállalati eszköz hello felhasználó jelentkezik be.</span><span class="sxs-lookup"><span data-stu-id="583b2-180">hello user is signing in on a corporate device.</span></span>
  - <span data-ttu-id="583b2-181">hello eszköz lett-e korábban csatlakoztatott tooyour Active Directory (AD) tartományhoz.</span><span class="sxs-lookup"><span data-stu-id="583b2-181">hello device has been previously joined tooyour Active Directory (AD) domain.</span></span>
  - <span data-ttu-id="583b2-182">hello eszközhöz tartozik egy közvetlen kapcsolat tooyour tartományvezérlőn (DC), vagy a hello vállalati vezetékes vagy vezeték nélküli hálózathoz, vagy távelérési kapcsolatot, például a VPN-kapcsolaton keresztül.</span><span class="sxs-lookup"><span data-stu-id="583b2-182">hello device has a direct connection tooyour Domain Controller (DC), either on hello corporate wired or wireless network or via a remote access connection, such as a VPN connection.</span></span>
  - <span data-ttu-id="583b2-183">Rendelkezik [hello szolgáltatás megkezdődött](##step-3-roll-out-the-feature) toothis felhasználói csoportházirend használatával.</span><span class="sxs-lookup"><span data-stu-id="583b2-183">You have [rolled out hello feature](##step-3-roll-out-the-feature) toothis user using Group Policy.</span></span>

<span data-ttu-id="583b2-184">tootest hello forgatókönyv ahol hello felhasználói belép csak hello felhasználónév, de nem hello jelszó:</span><span class="sxs-lookup"><span data-stu-id="583b2-184">tootest hello scenario where hello user enters only hello username, but not hello password:</span></span>
- <span data-ttu-id="583b2-185">Jelentkezzen be a *https://myapps.microsoft.com/* egy új titkos böngésző-munkamenetben.</span><span class="sxs-lookup"><span data-stu-id="583b2-185">Sign into *https://myapps.microsoft.com/* in a new private browser session.</span></span>

<span data-ttu-id="583b2-186">tootest hello forgatókönyv ahol hello a felhasználó nem rendelkezik a tooenter hello felhasználónév vagy hello jelszó:</span><span class="sxs-lookup"><span data-stu-id="583b2-186">tootest hello scenario where hello user doesn't have tooenter hello username or hello password:</span></span> 
- <span data-ttu-id="583b2-187">Jelentkezzen be a *https://myapps.microsoft.com/contoso.onmicrosoft.com* egy új titkos böngésző-munkamenetben.</span><span class="sxs-lookup"><span data-stu-id="583b2-187">Sign into *https://myapps.microsoft.com/contoso.onmicrosoft.com* in a new private browser session.</span></span> <span data-ttu-id="583b2-188">Cserélje le "*contoso*" a bérlő névvel.</span><span class="sxs-lookup"><span data-stu-id="583b2-188">Replace "*contoso*" with your tenant's name.</span></span>
- <span data-ttu-id="583b2-189">Jelentkezzen be vagy *https://myapps.microsoft.com/contoso.com* egy új titkos böngésző-munkamenetben.</span><span class="sxs-lookup"><span data-stu-id="583b2-189">Or sign into *https://myapps.microsoft.com/contoso.com* in a new private browser session.</span></span> <span data-ttu-id="583b2-190">Cserélje le "*contoso.com*" a bérlő ellenőrzött tartomány (nem összevont tartományhoz).</span><span class="sxs-lookup"><span data-stu-id="583b2-190">Replace "*contoso.com*" with a verified domain (not a federated domain) in your tenant.</span></span>

## <a name="step-5-roll-over-keys"></a><span data-ttu-id="583b2-191">5. lépés: A kulcs váltása</span><span class="sxs-lookup"><span data-stu-id="583b2-191">Step 5: Roll over keys</span></span>

<span data-ttu-id="583b2-192">2. lépésben az Azure AD Connect számítógépfiókot hoz létre (az Azure AD közti) az összes hello AD-erdőben, amelyiken engedélyezte a zökkenőmentes SSO.</span><span class="sxs-lookup"><span data-stu-id="583b2-192">In Step 2, Azure AD Connect creates computer accounts (representing Azure AD) in all hello AD forests on which you have enabled Seamless SSO.</span></span> <span data-ttu-id="583b2-193">Ismerje meg, a részletesebb [Itt](active-directory-aadconnect-sso-how-it-works.md).</span><span class="sxs-lookup"><span data-stu-id="583b2-193">Learn more in detail [here](active-directory-aadconnect-sso-how-it-works.md).</span></span> <span data-ttu-id="583b2-194">A nagyobb biztonság érdekében javasoljuk, hogy Ön gyakran váltása hello Kerberos visszafejtési kulcs esetében a számítógépfiókok.</span><span class="sxs-lookup"><span data-stu-id="583b2-194">For improved security, it is recommended that  you frequently roll over hello Kerberos decryption keys of these computer accounts.</span></span>

>[!IMPORTANT]
><span data-ttu-id="583b2-195">Nem kell toodo ebben a lépésben _azonnal_ hello szolgáltatás engedélyezése után.</span><span class="sxs-lookup"><span data-stu-id="583b2-195">You don't need toodo this step _immediately_ after you have enabled hello feature.</span></span> <span data-ttu-id="583b2-196">Váltása hello Kerberos visszafejtési kulcs esetében legalább 30 nap.</span><span class="sxs-lookup"><span data-stu-id="583b2-196">Roll over hello Kerberos decryption keys at least every 30 days.</span></span>

## <a name="next-steps"></a><span data-ttu-id="583b2-197">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="583b2-197">Next steps</span></span>

- <span data-ttu-id="583b2-198">[**Műszaki mélyreható** ](active-directory-aadconnect-sso-how-it-works.md) – Ez a funkció működésének megismerése.</span><span class="sxs-lookup"><span data-stu-id="583b2-198">[**Technical Deep Dive**](active-directory-aadconnect-sso-how-it-works.md) - Understand how this feature works.</span></span>
- <span data-ttu-id="583b2-199">[**Gyakran ismételt kérdések** ](active-directory-aadconnect-sso-faq.md) -kérdések toofrequently válaszol.</span><span class="sxs-lookup"><span data-stu-id="583b2-199">[**Frequently Asked Questions**](active-directory-aadconnect-sso-faq.md) - Answers toofrequently asked questions.</span></span>
- <span data-ttu-id="583b2-200">[**Hibaelhárítás** ](active-directory-aadconnect-troubleshoot-sso.md) -megtudhatja, hogyan tooresolve közös hello szolgáltatást érintő problémákat.</span><span class="sxs-lookup"><span data-stu-id="583b2-200">[**Troubleshoot**](active-directory-aadconnect-troubleshoot-sso.md) - Learn how tooresolve common issues with hello feature.</span></span>
- <span data-ttu-id="583b2-201">[**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) – új funkciókérések tárolásához.</span><span class="sxs-lookup"><span data-stu-id="583b2-201">[**UserVoice**](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) - For filing new feature requests.</span></span>
