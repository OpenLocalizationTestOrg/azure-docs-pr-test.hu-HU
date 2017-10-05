---
title: "Mi az a hozzáférési panel az Azure Active Directoryban? | Microsoft Docs"
description: "Ismerje meg, hogyan használható a hozzáférési panel (webböngésző, Android-alkalmazás, iPhone és iPad) férhetnek hozzá SaaS-alkalmazásokhoz való változatait."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: c0252d01-7e6e-4f79-a70e-600479577dfd
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: markvi
ms.reviewer: asteen
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: bd9066c251188c0f18fe1a9403baa2beaeeb987c
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="what-is-the-access-panel"></a><span data-ttu-id="50bc0-104">Mi az a hozzáférési panel?</span><span class="sxs-lookup"><span data-stu-id="50bc0-104">What is the access panel?</span></span>

<span data-ttu-id="50bc0-105">A hozzáférési panel egy webes portál.</span><span class="sxs-lookup"><span data-stu-id="50bc0-105">The access panel is a web-based portal.</span></span> <span data-ttu-id="50bc0-106">Lehetővé teszi a felhasználó munkahelyi vagy iskolai fiókkal az Azure Active Directoryban megtekintéséhez, és indítsa el a felhőalapú alkalmazások az Azure AD-rendszergazda rendelkezik hozzáféréssel őket.</span><span class="sxs-lookup"><span data-stu-id="50bc0-106">It enables a user with a work or school account in Azure Active Directory to view and start cloud-based applications an Azure AD administrator has granted them access to.</span></span> <span data-ttu-id="50bc0-107">Önkiszolgáló csoportkezelési és felügyeleti képességeit a hozzáférési panel keresztül is használható.</span><span class="sxs-lookup"><span data-stu-id="50bc0-107">You can also use self-service group and app management capabilities through the access panel.</span></span>

<span data-ttu-id="50bc0-108">A hozzáférési panel elkülönül az Azure-portálon, és nem Ön az Azure-előfizetésre does.</span><span class="sxs-lookup"><span data-stu-id="50bc0-108">The access panel is separate from the Azure portal and does not you to have an Azure subscription.</span></span>

![Hozzáférési panel][1]

<span data-ttu-id="50bc0-110">A hozzáférés lehetővé teszi, hogy bizonyos a profil beállításait, például a szerkesztése:</span><span class="sxs-lookup"><span data-stu-id="50bc0-110">The access panel enables you to edit some of your profile settings, including the ability to:</span></span>

- <span data-ttu-id="50bc0-111">A munkahelyi vagy iskolai fiókhoz tartozó jelszó módosítása</span><span class="sxs-lookup"><span data-stu-id="50bc0-111">Change the password associated with a work or school account</span></span>

- <span data-ttu-id="50bc0-112">Jelszó alaphelyzetbe állítása beállításainak szerkesztése</span><span class="sxs-lookup"><span data-stu-id="50bc0-112">Edit password reset settings</span></span>

- <span data-ttu-id="50bc0-113">Többtényezős hitelesítés (a rendszergazda lett rá szükséges fiókok) kapcsolatos ügyfél és a beállításokat szabályozó beállítások szerkesztése</span><span class="sxs-lookup"><span data-stu-id="50bc0-113">Edit contact and preference settings related to multi-factor authentication (for accounts that have been required to use it by an administrator)</span></span>

- <span data-ttu-id="50bc0-114">A fiók adatait, például a felhasználói Azonosítóját, másodlagos e-mail, és a mobil- és office telefonszámokat, és az eszközök megtekintése</span><span class="sxs-lookup"><span data-stu-id="50bc0-114">View account details, such as user ID, alternate email, and mobile and office phone numbers, and devices</span></span>

- <span data-ttu-id="50bc0-115">Megtekintheti, és indítsa el a felhőalapú alkalmazásokat, amelyek az Azure AD-rendszergazda engedélyezte őket a hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="50bc0-115">View and start cloud-based applications that the Azure AD administrator has granted them access to.</span></span> <span data-ttu-id="50bc0-116">A hozzáférési panel a a felhasználók szempontjából kapcsolatos további információkért lásd: a hozzáférési panelen.</span><span class="sxs-lookup"><span data-stu-id="50bc0-116">For more information about the access panel from the users’ perspective, see Using the access panel.</span></span> 

- <span data-ttu-id="50bc0-117">Önálló csoportok kezelése.</span><span class="sxs-lookup"><span data-stu-id="50bc0-117">Self-manage groups.</span></span> <span data-ttu-id="50bc0-118">Pontosabban, a rendszergazda létrehozhat és biztonsági csoportok kezelése és az Azure AD biztonsági csoporttagság kérése.</span><span class="sxs-lookup"><span data-stu-id="50bc0-118">More specifically, the administrator can create and manage security groups and request security group memberships in Azure AD.</span></span> <span data-ttu-id="50bc0-119">További információkért lásd: [önkiszolgáló csoportkezelési a felhasználók számára az Azure AD](active-directory-accessmanagement-self-service-group-management.md) és [saját csoportjai kezelését](active-directory-manage-groups.md).</span><span class="sxs-lookup"><span data-stu-id="50bc0-119">For more information, see [Self-service group management for users in Azure AD](active-directory-accessmanagement-self-service-group-management.md) and [Manage your groups](active-directory-manage-groups.md).</span></span>




## <a name="accessing-the-access-panel"></a><span data-ttu-id="50bc0-120">A hozzáférési panel elérése</span><span class="sxs-lookup"><span data-stu-id="50bc0-120">Accessing the access panel</span></span>

<span data-ttu-id="50bc0-121">A következő URL-címet egy webböngészőben felkeresésével érhető el a hozzáférési panel:`http://myapps.microsoft.com`</span><span class="sxs-lookup"><span data-stu-id="50bc0-121">You can access the access panel by visiting the following URL in a web browser: `http://myapps.microsoft.com`</span></span>

<span data-ttu-id="50bc0-122">Ha a bejelentkezési lapon konfigurált egyéni védjegyek, betöltése a védjegyek, az URL-cím végén a szervezet tartományához hozzáfűzésével:`http://myapps.microsoft.com/<your domain>.com`</span><span class="sxs-lookup"><span data-stu-id="50bc0-122">If you have custom branding configured for your sign-in page, you can load this branding by appending your organization’s domain to the end of the URL: `http://myapps.microsoft.com/<your domain>.com`</span></span>

<span data-ttu-id="50bc0-123">Ebben az esetben használhatja ki bármely aktív vagy ellenőrzött és érvényes tartománynevet, amely az Azure-portálon lett konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="50bc0-123">In this case, you can use any active or verified domain name that has been configured in your Azure portal.</span></span>

![A Wingtip Toys tartománynév][2]  

<span data-ttu-id="50bc0-125">Minden felhasználó számára lesz jelentkezzen be az Azure AD integrált alkalmazások URL-címét kell.</span><span class="sxs-lookup"><span data-stu-id="50bc0-125">You need to distribute the URL to all users who will sign in to applications that are integrated with Azure AD.</span></span>

## <a name="authentication"></a><span data-ttu-id="50bc0-126">Authentication</span><span class="sxs-lookup"><span data-stu-id="50bc0-126">Authentication</span></span>

<span data-ttu-id="50bc0-127">A hozzáférési panel elérését, keresztül a munkahelyi vagy iskolai fiókkal az Azure AD lehet hitelesíteni.</span><span class="sxs-lookup"><span data-stu-id="50bc0-127">To reach the access panel, you must be authenticated through a work or school account in Azure AD.</span></span> <span data-ttu-id="50bc0-128">Az Azure AD közvetlenül hitelesíthetők.</span><span class="sxs-lookup"><span data-stu-id="50bc0-128">You can be authenticated to Azure AD directly.</span></span> <span data-ttu-id="50bc0-129">Azt is megteheti Ha egy szervezet összevonási konfigurált Active Directory összevonási szolgáltatások (AD FS) vagy egyéb technológiák használatával, akkor hitelesítheti a Windows Server Active Directory.</span><span class="sxs-lookup"><span data-stu-id="50bc0-129">Alternatively, if an organization has configured federation by using Active Directory Federation Services (AD FS) or other technologies, you can be authenticated by Windows Server Active Directory.</span></span>

<span data-ttu-id="50bc0-130">Az Azure vagy Office 365-előfizetéssel rendelkezik, és az Azure-portálon vagy az Office 365 alkalmazás használja, ha az alkalmazások listáját láthatja nélkül aláíró újra be.</span><span class="sxs-lookup"><span data-stu-id="50bc0-130">If you have a subscription for Azure or Office 365 and you have been using the Azure portal or an Office 365 application, you can see the list of applications without signing-in again.</span></span> <span data-ttu-id="50bc0-131">Ha Ön nem hitelesített jelentkezzen be a felhasználónevet és jelszót a fiókhoz az Azure AD kéri.</span><span class="sxs-lookup"><span data-stu-id="50bc0-131">If you are are not authenticated you are prompted to sign in by using the username and password for your account in Azure AD.</span></span> <span data-ttu-id="50bc0-132">Ha a szervezet összevonási konfigurált, írja be a felhasználónevet is használhatók.</span><span class="sxs-lookup"><span data-stu-id="50bc0-132">If your organization has configured federation, typing the username is sufficient.</span></span>

<span data-ttu-id="50bc0-133">Amikor megtörténik, kezelheti az alkalmazásokat, amelyek a rendszergazda a könyvtárban van integrálva.</span><span class="sxs-lookup"><span data-stu-id="50bc0-133">When you are authenticated, you can interact with the applications that your administrator has integrated with the directory.</span></span> <span data-ttu-id="50bc0-134">Alkalmazások integrálása az Azure ad-vel kapcsolatban a [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval?](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="50bc0-134">To learn how to integrate applications with Azure AD, see [What is application access and single sign-on with Azure Active Directory?](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="web-browser-requirements"></a><span data-ttu-id="50bc0-135">Webböngészőkre vonatkozó követelmények</span><span class="sxs-lookup"><span data-stu-id="50bc0-135">Web browser requirements</span></span>

<span data-ttu-id="50bc0-136">Legalább a hozzáférési panel igényel, amely támogatja a JavaScript egy böngészőt, és CSS engedélyezte.</span><span class="sxs-lookup"><span data-stu-id="50bc0-136">At a minimum, the access panel requires a browser that supports JavaScript and has CSS enabled.</span></span> <span data-ttu-id="50bc0-137">A felhasználó bejelentkezhet a jelszó-alapú egyszeri bejelentkezést (SSO) alkalmazások a böngésző a hozzáférési panel bővítményét kell telepíteni.</span><span class="sxs-lookup"><span data-stu-id="50bc0-137">For the user to be signed in to applications through password-based single sign-on (SSO), the access panel extension must be installed in your browser.</span></span> <span data-ttu-id="50bc0-138">A bővítmény le automatikusan, amikor kiválaszt egy alkalmazást, amely jelszóalapú SSO van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="50bc0-138">The extension is downloaded automatically when you select an application that is configured for password-based SSO.</span></span>

<span data-ttu-id="50bc0-139">A hozzáférési panel bővítmény érhető el jelenleg Internet Explorer 8 és újabb verziók, Firefox, biztonsági és Chrome böngésző.</span><span class="sxs-lookup"><span data-stu-id="50bc0-139">The access panel extension is currently available for Internet Explorer 8 and later, Edge, Chrome, and Firefox browsers.</span></span>

## <a name="mobile-app-support"></a><span data-ttu-id="50bc0-140">Mobilalkalmazás-támogatás</span><span class="sxs-lookup"><span data-stu-id="50bc0-140">Mobile app support</span></span>

<span data-ttu-id="50bc0-141">Az Azure Active Directory ügyfélszolgálata közzéteszi az alkalmazásokat a mobilalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="50bc0-141">The Azure Active Directory team publishes the my apps mobile app.</span></span> <span data-ttu-id="50bc0-142">Az alkalmazás telepítésekor regisztrálhat jelszó alapú egyszeri bejelentkezés alkalmazásokhoz iOS és Android-eszközök.</span><span class="sxs-lookup"><span data-stu-id="50bc0-142">When you install the app, you can sign in to password-based SSO applications on iOS and Android devices.</span></span>

> [!NOTE]
> <span data-ttu-id="50bc0-143">Bejelentkezhet az Azure ad-val (például Salesforce, Google Apps, Dropbox, mezőben, Concur, Workday, Office 365, és több mint 70 mások) összevonási támogató alkalmazások szinte bármilyen böngészőben, bármilyen eszközön, anélkül, hogy a beépülő modul vagy mobil alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="50bc0-143">You can sign in to applications that support federation with Azure AD (including Salesforce, Google Apps, Dropbox, Box, Concur, Workday, Office 365, and more than 70 others) on virtually any web browser, on any device, without needing a plug-in or mobile app.</span></span> <span data-ttu-id="50bc0-144">Minden más [hozzáférési panel lép](https://myapps.microsoft.com/) is nincs szükség a mobileszközökön használandó alkalmazásokat mobilalkalmazáson keresztül.</span><span class="sxs-lookup"><span data-stu-id="50bc0-144">All other [access panel experiences](https://myapps.microsoft.com/) do also not require the my apps mobile app to be used on a mobile device.</span></span>
>
>

### <a name="my-apps-for-android"></a><span data-ttu-id="50bc0-145">A saját Android-alkalmazások</span><span class="sxs-lookup"><span data-stu-id="50bc0-145">My apps for Android</span></span>

<span data-ttu-id="50bc0-146">A saját Android-alkalmazások számítógépre Android 4.1 Android-verziót futtat és újabb rendszer.</span><span class="sxs-lookup"><span data-stu-id="50bc0-146">My apps for Android is supported on any Android device that is running Android version 4.1 and later.</span></span>  
<span data-ttu-id="50bc0-147">Elérhető a [Google Play áruház](https://play.google.com/store/apps/details?id=com.microsoft.myapps).</span><span class="sxs-lookup"><span data-stu-id="50bc0-147">It is available in the [Google Play store](https://play.google.com/store/apps/details?id=com.microsoft.myapps).</span></span>

![A saját Android-alkalmazások][3]   

### <a name="my-apps-for-iphone-and-ipad"></a><span data-ttu-id="50bc0-149">IPhone és iPad alkalmazásaimat</span><span class="sxs-lookup"><span data-stu-id="50bc0-149">My apps for iPhone and iPad</span></span>

<span data-ttu-id="50bc0-150">Az iOS-alkalmazások bármely iPhone vagy iPad futtató verziója iOS 7 és újabb verziók támogatják.</span><span class="sxs-lookup"><span data-stu-id="50bc0-150">My apps for iOS is supported on any iPhone or iPad that is running iOS version 7 and later.</span></span>  
<span data-ttu-id="50bc0-151">Elérhető a [Apple App Store](https://itunes.apple.com/us/app/my-apps-azure-active-directory/id824048653?mt=8).</span><span class="sxs-lookup"><span data-stu-id="50bc0-151">It is available in the [Apple App Store](https://itunes.apple.com/us/app/my-apps-azure-active-directory/id824048653?mt=8).</span></span>

![Az iOS-alkalmazások][4]    



## <a name="managed-browser-for-my-apps"></a><span data-ttu-id="50bc0-153">Felügyelt böngésző saját alkalmazások</span><span class="sxs-lookup"><span data-stu-id="50bc0-153">Managed browser for my apps</span></span>

<span data-ttu-id="50bc0-154">Az alkalmazások is integrálva van az Intune Managed Browser a.</span><span class="sxs-lookup"><span data-stu-id="50bc0-154">My apps is also integrated in the Intune Managed Browser.</span></span> <span data-ttu-id="50bc0-155">Az Intune Managed Browser iOS és Android-eszközök győződjön meg arról, hogy a mobileszközök adatainak biztonságos marad kulcsfontosságú szerepet játszik.</span><span class="sxs-lookup"><span data-stu-id="50bc0-155">The Intune Managed Browser for iOS and Android devices plays a key role in ensuring that data on mobile devices stays secure.</span></span> <span data-ttu-id="50bc0-156">Lehetővé teszi a biztonságosan megtekintheti, és keresse meg, amelyek a vállalati adatokat tartalmazhatnak, és biztonságos webböngésző élményt nyújt.</span><span class="sxs-lookup"><span data-stu-id="50bc0-156">It lets you safely view and navigate web pages that might contain company information, and provides a secure web-browsing experience.</span></span>  
<span data-ttu-id="50bc0-157">Látni alkalmazásaimat gyors elérését a Managed Browser kezdőlap és a könyvjelzőt, így kevesebb kattint, bármely az elérni kívánt alkalmazás eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="50bc0-157">You find quick access to my apps on your Managed Browser homepage and in your bookmarks, giving you fewer clicks to reach any application you want to access.</span></span>

<span data-ttu-id="50bc0-158">Elérhető a [Apple App Store](https://itunes.apple.com/us/app/microsoft-intune-managed-browser/id943264951?mt=8) és [Google Play áruház](https://play.google.com/store/apps/details?id=com.microsoft.intune.mam.managedbrowser&hl=en).</span><span class="sxs-lookup"><span data-stu-id="50bc0-158">It is available in the [Apple App Store](https://itunes.apple.com/us/app/microsoft-intune-managed-browser/id943264951?mt=8) and [Google Play Store](https://play.google.com/store/apps/details?id=com.microsoft.intune.mam.managedbrowser&hl=en).</span></span>

![Saját alkalmazások Mananged böngésző][5]    





## <a name="tips-for-testing-the-user-experience"></a><span data-ttu-id="50bc0-160">A felhasználói élmény tesztelési tippek</span><span class="sxs-lookup"><span data-stu-id="50bc0-160">Tips for testing the user experience</span></span>

<span data-ttu-id="50bc0-161">Ha egy Azure rendszergazdát és jelentkezett be az Azure portálra egy olyan fiókkal a címtárban, automatikusan jelentkezett a hozzáférési panelre, a jelenlegi fiókkal.</span><span class="sxs-lookup"><span data-stu-id="50bc0-161">If you are an Azure administrator and you are signed in to the Azure portal by using an account in the directory, you are automatically signed in to the access panel as your current account.</span></span> <span data-ttu-id="50bc0-162">Ebben az esetben tekintheti meg, hogy Önhöz rendelt összes alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="50bc0-162">In this case, you can see all applications that have been assigned to you.</span></span>

<span data-ttu-id="50bc0-163">**Tesztelésére, mint egy *különböző* felhasználói fiók:**</span><span class="sxs-lookup"><span data-stu-id="50bc0-163">**To test as a *different* user account:**</span></span>

1. <span data-ttu-id="50bc0-164">A felhasználó menü, az Azure-portálon vagy a hozzáférési panel jobb felső sarkában kattintson, majd **Kijelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="50bc0-164">Click the user menu in the upper-right corner of the Azure portal or the access panel, and then select **Sign Out**.</span></span> 
2. <span data-ttu-id="50bc0-165">Lépjen a [hozzáférési panel](http://myapps.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="50bc0-165">Go to the [access panel](http://myapps.microsoft.com).</span></span>
3. <span data-ttu-id="50bc0-166">A bejelentkezési lapon írja be a felhasználónév és a fiók jelszavát a vizsgálni kívánt könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="50bc0-166">On the sign-in page, type the username and password for the account in your directory you want to test.</span></span>


## <a name="starting-applications"></a><span data-ttu-id="50bc0-167">Alkalmazások elindítása</span><span class="sxs-lookup"><span data-stu-id="50bc0-167">Starting applications</span></span>

<span data-ttu-id="50bc0-168">Számos különböző típusú alkalmazások is jelennek meg a hozzáférési panel.</span><span class="sxs-lookup"><span data-stu-id="50bc0-168">Several types of applications can appear on the access panel.</span></span>

### <a name="office-365-applications"></a><span data-ttu-id="50bc0-169">Office 365-alkalmazások</span><span class="sxs-lookup"><span data-stu-id="50bc0-169">Office 365 applications</span></span>

<span data-ttu-id="50bc0-170">Ha a szervezet által használt Office 365-alkalmazásokhoz, és rendelkezik licenccel, az Office 365-alkalmazások jelennek meg a hozzáférési panel.</span><span class="sxs-lookup"><span data-stu-id="50bc0-170">If your organization is using Office 365 applications and you are licensed for them, the Office 365 applications appear on your access panel.</span></span>

<span data-ttu-id="50bc0-171">Ha egy alkalmazás csempe az Office 365 alkalmazás gombra kattint, Ön átirányítja az alkalmazásba, és automatikusan megtörténik a.</span><span class="sxs-lookup"><span data-stu-id="50bc0-171">When you click an application tile for an Office 365 application, you are redirected to the application and automatically signed in.</span></span>

### <a name="microsoft-and-third-party-applications-configured-with-federation-based-sso"></a><span data-ttu-id="50bc0-172">Összevonási-alapú egyszeri bejelentkezési modellel konfigurált a Microsoft és külső alkalmazások</span><span class="sxs-lookup"><span data-stu-id="50bc0-172">Microsoft and third-party applications configured with federation-based SSO</span></span>

<span data-ttu-id="50bc0-173">A rendszergazda adhat hozzá alkalmazásokat az Azure-portálon az Active Directory szakaszában az SSO módban **az Azure AD az egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="50bc0-173">Your administrator can add applications in the Active Directory section of the Azure portal with the SSO mode set to **Azure AD Single Sign-On**.</span></span> <span data-ttu-id="50bc0-174">Csak megtekintheti ezeket az alkalmazásokat, ha a rendszergazda explicit módon adott az alkalmazások elérését.</span><span class="sxs-lookup"><span data-stu-id="50bc0-174">You can only see these applications if your administrator has explicitly granted you access to the applications.</span></span>

<span data-ttu-id="50bc0-175">Ezen alkalmazások egyikét mozaikokra kattintva Ön átirányítva, és automatikusan bejelentkeztetjük az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="50bc0-175">When you click a tile for one of these applications, you are redirected and automatically signed in to the application.</span></span>

### <a name="password-based-sso-without-identity-provisioning"></a><span data-ttu-id="50bc0-176">Egyszeri bejelentkezés jelszó alapú identitás kiépítés nélkül</span><span class="sxs-lookup"><span data-stu-id="50bc0-176">Password-based SSO without identity provisioning</span></span>

<span data-ttu-id="50bc0-177">A rendszergazda adhat hozzá alkalmazásokat az Azure-portálon az Active Directory szakaszában az SSO módban **jelszó-alapú egyszeri bejelentkezést**.</span><span class="sxs-lookup"><span data-stu-id="50bc0-177">Your administrator can add applications in the Active Directory section of the Azure portal with the SSO mode set to **Password-based Single Sign-On**.</span></span> <span data-ttu-id="50bc0-178">A címtárban szereplő összes felhasználó láthatja az összes olyan alkalmazások, amelyek ebben a módban vannak konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="50bc0-178">All users in the directory can see all applications that have been configured in this mode.</span></span>

<span data-ttu-id="50bc0-179">Először, kattint egy ezeket az alkalmazásokat, egy csempe kéri a beépülő modul jelszó SSO telepítéséhez az Internet Explorer vagy a Chrome.</span><span class="sxs-lookup"><span data-stu-id="50bc0-179">The first time, you click a tile for one of these applications, you are prompted to install the Password SSO plug-in for Internet Explorer or Chrome.</span></span> <span data-ttu-id="50bc0-180">A telepítés előfordulhat, hogy indítsa újra a webböngészőt.</span><span class="sxs-lookup"><span data-stu-id="50bc0-180">The installation might require you to restart your web browser.</span></span> <span data-ttu-id="50bc0-181">Ha a hozzáférési panelre való visszatéréshez, és kattintson az alkalmazás csempéjére újra, felhasználónevet és jelszót az alkalmazás kéri.</span><span class="sxs-lookup"><span data-stu-id="50bc0-181">When you return to the access panel and click the application tile again, you are prompted for a username and password for the application.</span></span> <span data-ttu-id="50bc0-182">Amikor a felhasználónevet és jelszót adta meg, ezek a hitelesítő adatok biztonságos tárolása és kapcsolódik ahhoz a fiókhoz az Azure AD.</span><span class="sxs-lookup"><span data-stu-id="50bc0-182">When you have entered your username and password, these credentials are securely stored and linked to your account in Azure AD.</span></span>

<span data-ttu-id="50bc0-183">A következő alkalommal alkalmazás csempére kattintva automatikusan jelentkezett az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="50bc0-183">The next time you click the application tile, you are automatically signed in to the application.</span></span>  
<span data-ttu-id="50bc0-184">Írja be újra a hitelesítő adatait, és vagy a jelszó SSO beépülő modul telepítése nincs.</span><span class="sxs-lookup"><span data-stu-id="50bc0-184">You don't have to enter your credentials again and or install the Password SSO plug-in.</span></span>

<span data-ttu-id="50bc0-185">Ha a hitelesítő adatait a harmadik fél célalkalmazásban módosult, frissíteni kell az Azure AD-ben tárolt hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="50bc0-185">If your credentials have changed in the target third-party application, you must also update your credentials that are stored in Azure AD.</span></span> 

<span data-ttu-id="50bc0-186">**Hitelesítő adatainak frissítése:**</span><span class="sxs-lookup"><span data-stu-id="50bc0-186">**To update credentials:**</span></span>

1. <span data-ttu-id="50bc0-187">Válassza ki az alkalmazás csempe a ikonra.</span><span class="sxs-lookup"><span data-stu-id="50bc0-187">Select the icon on the application tile.</span></span>
2. <span data-ttu-id="50bc0-188">Válassza ki **hitelesítő adatainak frissítése** újbóli a felhasználónevet és jelszót az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="50bc0-188">Select **update credentials** to reenter the username and password for the application.</span></span>


### <a name="password-based-sso-with-identity-provisioning"></a><span data-ttu-id="50bc0-189">Jelszó-alapú egyszeri bejelentkezés identitás kiépítése</span><span class="sxs-lookup"><span data-stu-id="50bc0-189">Password-based SSO with identity provisioning</span></span>

<span data-ttu-id="50bc0-190">A rendszergazda adhat hozzá alkalmazásokat a **Active Directory** az Azure-portálon az SSO módban szakasza **jelszó-alapú egyszeri bejelentkezést**, identitás kiépítés együtt.</span><span class="sxs-lookup"><span data-stu-id="50bc0-190">Your administrator can add applications in the **Active Directory** section of the Azure portal with the SSO mode set to **Password-based Single Sign-On**, along with identity provisioning.</span></span>

<span data-ttu-id="50bc0-191">Az első alkalommal kattint, az alkalmazás csempéjére a hozzá tartozó egyik ezeket az alkalmazásokat, telepítendő kéri a **jelszó SSO beépülő modul az Internet Explorer vagy a Chrome**.</span><span class="sxs-lookup"><span data-stu-id="50bc0-191">The first time, you click an application tile for one of these applications, you are prompted to install the **Password SSO plug-in for Internet Explorer or Chrome**.</span></span> <span data-ttu-id="50bc0-192">A telepítés előfordulhat, hogy indítsa újra a webböngészőt.</span><span class="sxs-lookup"><span data-stu-id="50bc0-192">The installation might require you to restart your web browser.</span></span>  
<span data-ttu-id="50bc0-193">Ha a hozzáférési panelre való visszatéréshez, és kattintson ismét az alkalmazás csempe, automatikusan jelentkezett az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="50bc0-193">When you return to the access panel and click the application tile again, you are automatically signed in to the application.</span></span>

<span data-ttu-id="50bc0-194">Egyes alkalmazások szükség lehet az első bejelentkezés jelszó módosítása.</span><span class="sxs-lookup"><span data-stu-id="50bc0-194">Some applications might require you to change your password on the first sign-in.</span></span> <span data-ttu-id="50bc0-195">Ha a hitelesítő adatait a harmadik fél célalkalmazásban módosult, frissíteni kell az Azure AD-ben tárolt hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="50bc0-195">If your credentials have changed in the target third-party application, you must also update the credentials that are stored in Azure AD.</span></span> 

<span data-ttu-id="50bc0-196">**Hitelesítő adatainak frissítése:**</span><span class="sxs-lookup"><span data-stu-id="50bc0-196">**To update credentials:**</span></span>

1. <span data-ttu-id="50bc0-197">Válassza ki az alkalmazás csempe a ikonra.</span><span class="sxs-lookup"><span data-stu-id="50bc0-197">Select the icon on the application tile.</span></span>
2. <span data-ttu-id="50bc0-198">Válassza ki **hitelesítő adatainak frissítése** újbóli a felhasználónevet és jelszót az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="50bc0-198">Select **update credentials** to reenter the username and password for the application.</span></span>


### <a name="application-with-existing-sso-solutions"></a><span data-ttu-id="50bc0-199">A már meglévő SSO megoldásaival alkalmazás</span><span class="sxs-lookup"><span data-stu-id="50bc0-199">Application with existing SSO solutions</span></span>

<span data-ttu-id="50bc0-200">Egyszeri bejelentkezés konfigurálása az alkalmazáshoz, az Azure-portálon biztosít a harmadik lehetőség nevű **meglévő egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="50bc0-200">To configure SSO for an application, the Azure portal provides a third option called **Existing Single Sign-On**.</span></span> <span data-ttu-id="50bc0-201">Ez a beállítás lehetővé teszi, hogy a rendszergazdát, hogy egy alkalmazás mutató hivatkozás létrehozásához, és helyezze el a kiválasztott felhasználók hozzáférési panelje.</span><span class="sxs-lookup"><span data-stu-id="50bc0-201">This option enables your administrator to create a link to an application and place it on the access panel for selected users.</span></span>

<span data-ttu-id="50bc0-202">Például ha egy alkalmazás a felhasználók hitelesítése az AD FS 2.0 használatával van konfigurálva, a rendszergazda végezheti el a **meglévő egyszeri bejelentkezés** létrehozhat egy hivatkozásra a hozzáférési panelen.</span><span class="sxs-lookup"><span data-stu-id="50bc0-202">For example, if an application is configured to authenticate users by using AD FS 2.0, your administrator can use the **Existing Single Sign-On** option to create a link to it on the access panel.</span></span> <span data-ttu-id="50bc0-203">A hivatkozás fér hozzá, amikor a rendszer hitelesíti az AD FS 2.0-s vagy bármilyen meglévő egyszeri bejelentkezési megoldással az alkalmazás maga biztosítja.</span><span class="sxs-lookup"><span data-stu-id="50bc0-203">When you access the link, you are authenticated through AD FS 2.0 or whatever existing SSO solution the application provides.</span></span>


## <a name="next-steps"></a><span data-ttu-id="50bc0-204">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="50bc0-204">Next steps</span></span>

- <span data-ttu-id="50bc0-205">Az Alkalmazáskezelés kapcsolódó témakörök listáját, olvassa el a [cikk index az Azure Active Directoryban Alkalmazáskezelés](active-directory-apps-index.md).</span><span class="sxs-lookup"><span data-stu-id="50bc0-205">To see a list of all topics that are related to application management, see the [article index for application management in Azure Active Directory](active-directory-apps-index.md).</span></span>
 
- <span data-ttu-id="50bc0-206">A Szolgáltatottszoftver-alkalmazás integrálja az Azure AD, lásd: a [SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása](active-directory-saas-tutorial-list.md).</span><span class="sxs-lookup"><span data-stu-id="50bc0-206">To learn how to integrate a SaaS app into Azure AD, see the [list of tutorials on how to integrate SaaS apps](active-directory-saas-tutorial-list.md).</span></span>
 
- <span data-ttu-id="50bc0-207">Alkalmazások kezelése az Azure AD kapcsolatos további tudnivalókért tekintse meg a [bemutatása az Azure Active Directoryval egyszeri bejelentkezés és kezelését alkalmazás eléréséhez](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="50bc0-207">To learn more about managing apps with Azure AD, see the [introduction to single sign-on and managing app access with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>
 
- <span data-ttu-id="50bc0-208">A felhasználók átadása kapcsolatos további információkért lásd: [automatizálhatja a felhasználó kiépítésének és megszüntetésének biztosítása SaaS-alkalmazásokhoz való](active-directory-saas-app-provisioning.md).</span><span class="sxs-lookup"><span data-stu-id="50bc0-208">To learn more about user provisioning, see [automate user provisioning and deprovisioning to SaaS applications](active-directory-saas-app-provisioning.md).</span></span>

<!--Image references-->
[1]: ./media/active-directory-saas-access-panel-introduction/01.png
[2]: ./media/active-directory-saas-access-panel-introduction/02.png
[3]: ./media/active-directory-saas-access-panel-introduction/03.png
[4]: ./media/active-directory-saas-access-panel-introduction/04.png
[5]: ./media/active-directory-saas-access-panel-introduction/05.png
