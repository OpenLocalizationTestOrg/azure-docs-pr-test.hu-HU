---
title: "Bejelentkezés egy alkalmazás a hozzáférési panelen igényelheti problémák |} Microsoft Docs"
description: "Hibaelhárítás állít ki a Microsoft Azure AD hozzáférési panelre a myapps.microsoft.com az alkalmazáshoz való hozzáférés"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.reviewer: japere
ms.openlocfilehash: 188a00db59b0aa8d26facc678fb52d96272183b6
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="problems-signing-in-to-an-application-from-the-access-panel"></a><span data-ttu-id="b5ae5-103">Bejelentkezés egy alkalmazás a hozzáférési panelen igényelheti problémák</span><span class="sxs-lookup"><span data-stu-id="b5ae5-103">Problems signing in to an application from the access panel</span></span>

<span data-ttu-id="b5ae5-104">A hozzáférési Panel egy webes portál, amely lehetővé teszi a felhasználó munkahelyi vagy iskolai fiókkal az Azure Active Directory (Azure AD) a megtekintése, és indítsa el a felhőalapú alkalmazások, hogy az Azure AD-rendszergazda engedélyezte őket hozzáférés annak.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-104">The Access Panel is a web-based portal which enables a user with a work or school account in Azure Active Directory (Azure AD) to view and start cloud-based applications that the Azure AD administrator has granted them access to.</span></span> 

<span data-ttu-id="b5ae5-105">Ezeket az alkalmazásokat úgy vannak konfigurálva, az Azure AD portálon a felhasználó nevében.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-105">These applications are configured on behalf of the user in the Azure AD portal.</span></span> <span data-ttu-id="b5ae5-106">Az alkalmazás megfelelően konfigurálva legyen, és a felhasználó vagy egy csoport, a felhasználó tagja a hozzáférési panelen az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-106">The application must be configured properly and assigned to the user or a group the user is a member of to see the application in the Access Panel.</span></span>

<span data-ttu-id="b5ae5-107">Alkalmazások azért jelent meg a felhasználó típusa a következő kategóriákba tartoznak:</span><span class="sxs-lookup"><span data-stu-id="b5ae5-107">The type of apps a user may be seeing fall in the following categories:</span></span>

-   <span data-ttu-id="b5ae5-108">Office 365-alkalmazások</span><span class="sxs-lookup"><span data-stu-id="b5ae5-108">Office 365 Applications</span></span>

-   <span data-ttu-id="b5ae5-109">Összevonási-alapú egyszeri bejelentkezési modellel konfigurált a Microsoft és külső alkalmazások</span><span class="sxs-lookup"><span data-stu-id="b5ae5-109">Microsoft and third-party applications configured with federation-based SSO</span></span>

-   <span data-ttu-id="b5ae5-110">Jelszó-alapú egyszeri bejelentkezés alkalmazásokhoz</span><span class="sxs-lookup"><span data-stu-id="b5ae5-110">Password-based SSO applications</span></span>

-   <span data-ttu-id="b5ae5-111">A már meglévő SSO megoldásaival alkalmazások</span><span class="sxs-lookup"><span data-stu-id="b5ae5-111">Applications with existing SSO solutions</span></span>

## <a name="general-issues-to-check-first"></a><span data-ttu-id="b5ae5-112">Először ellenőrizze a általános problémák</span><span class="sxs-lookup"><span data-stu-id="b5ae5-112">General issues to check first</span></span>

-   <span data-ttu-id="b5ae5-113">Győződjön meg arról, hogy a használatával egy **böngésző** , amely megfelel a hozzáférési Panel minimális követelményeinek.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-113">Make sure your using a **browser** that meets the minimum requirements for the Access Panel.</span></span>

-   <span data-ttu-id="b5ae5-114">Győződjön meg arról, hogy a felhasználó böngészőjéből az alkalmazás URL-CÍMÉT hozzáadta a **megbízható helyek**.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-114">Make sure the user’s browser has added the URL of the application to its **trusted sites**.</span></span>

-   <span data-ttu-id="b5ae5-115">Ellenőrizze, hogy az alkalmazás **konfigurált** megfelelően.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-115">Make sure to check the application is **configured** correctly.</span></span>

-   <span data-ttu-id="b5ae5-116">Győződjön meg arról, hogy a felhasználói fiók **engedélyezett** a indított bejelentkezések.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-116">Make sure the user’s account is **enabled** for sign ins.</span></span>

-   <span data-ttu-id="b5ae5-117">Győződjön meg arról, hogy a felhasználói fiók **nincs zárolva.**</span><span class="sxs-lookup"><span data-stu-id="b5ae5-117">Make sure the user’s account is **not locked out.**</span></span>

-   <span data-ttu-id="b5ae5-118">Győződjön meg arról, hogy a felhasználó **nem lejárt vagy elfelejtett jelszó.**</span><span class="sxs-lookup"><span data-stu-id="b5ae5-118">Make sure the user’s **password is not expired or forgotten.**</span></span>

-   <span data-ttu-id="b5ae5-119">Győződjön meg arról, hogy **multi-factor Authentication** nem blokkolja a hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-119">Make sure **Multi-Factor Authentication** is not blocking user access.</span></span>

-   <span data-ttu-id="b5ae5-120">Győződjön meg arról, hogy egy **feltételes hozzáférési házirend** vagy **Identity Protection** házirend nem blokkolja a hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-120">Make sure a **Conditional Access policy** or **Identity Protection** policy is not blocking user access.</span></span>

-   <span data-ttu-id="b5ae5-121">Győződjön meg arról, hogy a felhasználó **hitelesítési kapcsolattartási adatai** esetén a multi-factor Authentication vagy feltételes hozzáférési házirendek kényszerítését dátum.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-121">Make sure that a user’s **authentication contact info** is up to date to allow Multi-Factor Authentication or Conditional Access policies to be enforced.</span></span>

-   <span data-ttu-id="b5ae5-122">A győződjön meg arról is megpróbálja törlésével a böngésző cookie-kat, majd próbálkozzon újból bejelentkezni.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-122">Make sure to also try clearing your browser’s cookies and trying to sign in again.</span></span>

## <a name="meeting-browser-requirements-for-the-access-panel"></a><span data-ttu-id="b5ae5-123">A hozzáférési Panel értekezlet böngészőre vonatkozó követelményei</span><span class="sxs-lookup"><span data-stu-id="b5ae5-123">Meeting browser requirements for the Access Panel</span></span>

<span data-ttu-id="b5ae5-124">A hozzáférési Panel igényel, amely támogatja a JavaScript egy böngészőt, és CSS engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-124">The Access Panel requires a browser that supports JavaScript and has CSS enabled.</span></span> <span data-ttu-id="b5ae5-125">Jelszó-alapú egyszeri bejelentkezést (SSO) használ a hozzáférési panelen, a hozzáférési Panel bővítményt telepíteni kell a felhasználó böngészőben.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-125">To use password-based single sign-on (SSO) in the Access Panel, the Access Panel extension must be installed in the user’s browser.</span></span> <span data-ttu-id="b5ae5-126">A bővítmény le automatikusan, ha a felhasználó megadja egy alkalmazás, amely jelszóalapú SSO van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-126">This extension is downloaded automatically when a user selects an application that is configured for password-based SSO.</span></span>

<span data-ttu-id="b5ae5-127">Jelszó-alapú egyszeri bejelentkezéshez a végfelhasználó böngészőkkel lehet:</span><span class="sxs-lookup"><span data-stu-id="b5ae5-127">For password-based SSO, the end user’s browsers can be:</span></span>

-   <span data-ttu-id="b5ae5-128">Internet Explorer 8, 9, 10, 11 – a Windows 7 vagy újabb</span><span class="sxs-lookup"><span data-stu-id="b5ae5-128">Internet Explorer 8, 9, 10, 11 -- on Windows 7 or later</span></span>

-   <span data-ttu-id="b5ae5-129">Peremhálózati Windows 10 évforduló Edition vagy újabb</span><span class="sxs-lookup"><span data-stu-id="b5ae5-129">Edge on Windows 10 Anniversary Edition or later</span></span>

-   <span data-ttu-id="b5ae5-130">Chrome – A Windows 7 vagy újabb, és MacOS X rendszeren vagy újabb</span><span class="sxs-lookup"><span data-stu-id="b5ae5-130">Chrome -- on Windows 7 or later, and on MacOS X or later</span></span>

-   <span data-ttu-id="b5ae5-131">Firefox 26.0 vagy újabb – a Windows XP SP2 vagy újabb, és a Mac OS X 10,6 vagy újabb verzió</span><span class="sxs-lookup"><span data-stu-id="b5ae5-131">Firefox 26.0 or later -- on Windows XP SP2 or later, and on Mac OS X 10.6 or later</span></span>

## <a name="how-to-install-the-access-panel-browser-extension"></a><span data-ttu-id="b5ae5-132">A hozzáférési Panel bővítmény telepítése</span><span class="sxs-lookup"><span data-stu-id="b5ae5-132">How to install the Access Panel Browser extension</span></span>

<span data-ttu-id="b5ae5-133">A hozzáférési Panel bővítmény telepítéséhez kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="b5ae5-133">To install the Access Panel Browser extension, follow the steps below:</span></span>

1.  <span data-ttu-id="b5ae5-134">Nyissa meg a [hozzáférési Panel](https://myapps.microsoft.com) valamelyik támogatott böngészők és való bejelentkezést egy **felhasználói** az Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-134">Open the [Access Panel](https://myapps.microsoft.com) in one of the supported browsers and sign in as a **user** in your Azure AD.</span></span>

2.  <span data-ttu-id="b5ae5-135">Kattintson egy **jelszó-SSO alkalmazás** a hozzáférési panelen.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-135">Click a **password-SSO application** in the Access Panel.</span></span>

3.  <span data-ttu-id="b5ae5-136">Válassza ki a szoftver telepítéséhez megkérdezi **telepítés**.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-136">In the prompt asking to install the software, select **Install Now**.</span></span>

4.  <span data-ttu-id="b5ae5-137">A böngésző alapján kell irányítani a letöltési hivatkozás.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-137">Based on your browser you be directed to the download link.</span></span> <span data-ttu-id="b5ae5-138">**Adja hozzá** az bővítményt, hogy a böngészőben.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-138">**Add** the extension to your browser.</span></span>

5.  <span data-ttu-id="b5ae5-139">Ha a böngésző, válassza ki vagy **engedélyezése** vagy **engedélyezése** a bővítményt.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-139">If your browser asks, select to either **Enable** or **Allow** the extension.</span></span>

6.  <span data-ttu-id="b5ae5-140">Korábban telepítve, **indítsa újra a** a böngésző-munkamenetet.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-140">Once installed, **restart** your browser session.</span></span>

7.  <span data-ttu-id="b5ae5-141">Jelentkezzen be a hozzáférési panelre, és tekintse meg, ha **indítása** a jelszó-egyszeri bejelentkezés alkalmazásokhoz</span><span class="sxs-lookup"><span data-stu-id="b5ae5-141">Sign in into the Access Panel and see if you can **launch** your password-SSO applications</span></span>

<span data-ttu-id="b5ae5-142">Az alábbi közvetlen hivatkozások közül a Chrome és a peremhálózati is letöltheti a bővítmény:</span><span class="sxs-lookup"><span data-stu-id="b5ae5-142">You may also download the extension for Chrome and Edge from the direct links below:</span></span>

-   [<span data-ttu-id="b5ae5-143">Chrome hozzáférési Panel bővítmény</span><span class="sxs-lookup"><span data-stu-id="b5ae5-143">Chrome Access Panel Extension</span></span>](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

-   [<span data-ttu-id="b5ae5-144">Peremhálózati hozzáférési Panel bővítmény</span><span class="sxs-lookup"><span data-stu-id="b5ae5-144">Edge Access Panel Extension</span></span>](https://www.microsoft.com/store/apps/9pc9sckkzk84)

## <a name="how-to-configure-federated-single-sign-on-for-an-azure-ad-gallery-application"></a><span data-ttu-id="b5ae5-145">Összevont egyszeri bejelentkezés az Azure AD gyűjtemény alkalmazás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b5ae5-145">How to configure federated single sign-on for an Azure AD gallery application</span></span>

<span data-ttu-id="b5ae5-146">Vállalati egyszeri bejelentkezésre alkalmas engedélyezve van az Azure AD-dokumentumtárban minden alkalmazás rendelkezik elérhető részletes oktatóanyaga.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-146">All application in the Azure AD gallery enabled with Enterprise Single Sign-On capability has a step-by-step tutorial available.</span></span> <span data-ttu-id="b5ae5-147">Érheti el a [SaaS-alkalmazásokhoz az Azure Active Directoryval integrációjával kapcsolatos bemutatók felsorolása](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/) részletes lépésenkénti útmutatót.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-147">You can access the [List of tutorials on how to integrate SaaS apps with Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/) for a detail step-by-step guidance.</span></span>

<span data-ttu-id="b5ae5-148">Egy alkalmazás kell az Azure AD-galériából konfigurálása:</span><span class="sxs-lookup"><span data-stu-id="b5ae5-148">To configure an application from the Azure AD gallery you need to:</span></span>

-   [<span data-ttu-id="b5ae5-149">Alkalmazás hozzáadása az Azure AD-galériából</span><span class="sxs-lookup"><span data-stu-id="b5ae5-149">Add an application from the Azure AD gallery</span></span>](#add-an-application)

-   [<span data-ttu-id="b5ae5-150">Az alkalmazás metaadatai értékeinek konfigurálása az Azure AD (bejelentkezési URL-cím, az azonosítója, válasz URL-címe)</span><span class="sxs-lookup"><span data-stu-id="b5ae5-150">Configure the application’s metadata values in Azure AD (Sign on URL, Identifier, Reply URL)</span></span>](#configure-single-sign-on-for-an-application-from-the-azure-ad-gallery)

-   [<span data-ttu-id="b5ae5-151">Válassza ki a felhasználói azonosítóját, és elküldi az alkalmazás felhasználói attribútumok hozzáadása</span><span class="sxs-lookup"><span data-stu-id="b5ae5-151">Select User Identifier and add user attributes to be sent to the application</span></span>](#select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application)

-   [<span data-ttu-id="b5ae5-152">Az Azure AD-metaadatok és a tanúsítvány lekérése</span><span class="sxs-lookup"><span data-stu-id="b5ae5-152">Retrieve Azure AD metadata and certificate</span></span>](#download-the-azure-ad-metadata-or-certificate)

-   [<span data-ttu-id="b5ae5-153">Konfigurálja az Azure AD metaadatok értékeket az alkalmazásban (bejelentkezési URL-címet, a kibocsátó, a kijelentkezési URL-cím és a tanúsítvány)</span><span class="sxs-lookup"><span data-stu-id="b5ae5-153">Configure Azure AD metadata values in the application (Sign on URL, Issuer, Logout URL and certificate)</span></span>](#configure-single-sign-on-for-an-application-from-the-azure-ad-gallery)

-   [<span data-ttu-id="b5ae5-154">Az alkalmazás felhasználók hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="b5ae5-154">Assign users to the application</span></span>](#assign-users-to-the-application)

### <a name="add-an-application-from-the-azure-ad-gallery"></a><span data-ttu-id="b5ae5-155">Alkalmazás hozzáadása az Azure AD-galériából</span><span class="sxs-lookup"><span data-stu-id="b5ae5-155">Add an application from the Azure AD gallery</span></span>

<span data-ttu-id="b5ae5-156">Hozzáadhat egy alkalmazást az Azure AD-gyűjteményből, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="b5ae5-156">To add an application from the Azure AD Gallery, follow the steps below:</span></span>

1.  <span data-ttu-id="b5ae5-157">Nyissa meg a [Azure Portal](https://portal.azure.com) , és jelentkezzen be egy **globális rendszergazda** vagy **társadminisztrátornak**</span><span class="sxs-lookup"><span data-stu-id="b5ae5-157">Open the [Azure Portal](https://portal.azure.com) and sign in as a **Global Administrator** or **Co-admin**</span></span>

2.  <span data-ttu-id="b5ae5-158">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-158">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="b5ae5-159">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-159">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="b5ae5-160">Kattintson a **vállalati alkalmazások** az Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-160">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="b5ae5-161">Kattintson a **Hozzáadás** gombra a jobb felső sarkában a **vállalati alkalmazások** panel</span><span class="sxs-lookup"><span data-stu-id="b5ae5-161">click the **Add** button at the top-right corner on the **Enterprise Applications** blade</span></span>

6.  <span data-ttu-id="b5ae5-162">Az a **adjon meg egy nevet** a szövegmező a **hozzáadása a gyűjteményből** területen írja be az alkalmazás nevét.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-162">In the **Enter a name** textbox from the **Add from the gallery** section, type the name of the application.</span></span>

7.  <span data-ttu-id="b5ae5-163">Válassza ki az alkalmazást, az egyszeri bejelentkezés konfigurálni szeretné.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-163">Select the application you want to configure for single sign-on.</span></span>

8.  <span data-ttu-id="b5ae5-164">Ad hozzá az alkalmazást, mielőtt a nevét módosíthatja a **neve** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-164">Before adding the application, you can change its name from the **Name** textbox.</span></span>

9.  <span data-ttu-id="b5ae5-165">Kattintson a **Hozzáadás** gombra, vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-165">Click **Add** button, to add the application.</span></span>

<span data-ttu-id="b5ae5-166">Egy rövid időszak után el az alkalmazás konfigurációs panelen láthatók.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-166">After a short period, you be able to see the application’s configuration blade.</span></span>

### <a name="configure-single-sign-on-for-an-application-from-the-azure-ad-gallery"></a><span data-ttu-id="b5ae5-167">Egyszeri bejelentkezés egy alkalmazás az Azure AD-galériából konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b5ae5-167">Configure single sign-on for an application from the Azure AD gallery</span></span>

<span data-ttu-id="b5ae5-168">Egyszeri bejelentkezés egy alkalmazás konfigurálásához kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="b5ae5-168">To configure single sign-on for an application, follow the steps below:</span></span>

1.  <span data-ttu-id="b5ae5-169"><span id="_Hlk477187909" class="anchor"><span id="_Hlk477001983" class="anchor"></span></span>Nyissa meg a [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**</span><span class="sxs-lookup"><span data-stu-id="b5ae5-169"><span id="_Hlk477187909" class="anchor"><span id="_Hlk477001983" class="anchor"></span></span>Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="b5ae5-170">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-170">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="b5ae5-171">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-171">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="b5ae5-172">Kattintson a **vállalati alkalmazások** az Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-172">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="b5ae5-173">Kattintson a **összes alkalmazás** az alkalmazások listájának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-173">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="b5ae5-174">Ha azt szeretné, hogy itt megjelennek az alkalmazás nem látja, használja a **szűrő** vezérlő tetején a **összes alkalmazások listáját** és állítsa be a **megjelenítése** lehetőséggel **összes alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="b5ae5-174">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="b5ae5-175">Válassza ki az egyszeri bejelentkezés konfigurálni kívánt alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-175">Select the application you want to configure single sign-on.</span></span>

7.  <span data-ttu-id="b5ae5-176">Ha az alkalmazás betölt, kattintson a **egyszeri bejelentkezés** az alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-176">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="b5ae5-177">Válassza ki **SAML-alapú bejelentkezés** a a **mód** legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-177">Select **SAML-based Sign-on** from the **Mode** dropdown.</span></span>

9.  <span data-ttu-id="b5ae5-178">Adja meg a szükséges értékeket a **tartomány és az URL-címeket.**</span><span class="sxs-lookup"><span data-stu-id="b5ae5-178">Enter the required values in **Domain and URLs.**</span></span> <span data-ttu-id="b5ae5-179">Ezeket az értékeket az alkalmazás gyártójának szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-179">You should get these values from the application vendor.</span></span>

   1. <span data-ttu-id="b5ae5-180">Az alkalmazás beállítása a Szolgáltató által kezdeményezett egyszeri Bejelentkezést, a bejelentkezési URL-címen nem kötelező megadni.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-180">To configure the application as SP-initiated SSO, the Sign on URL it’s a required value.</span></span> <span data-ttu-id="b5ae5-181">Egyes alkalmazások azonosító is kötelező megadni.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-181">For some applications, the Identifier is also a required value.</span></span>

   2. <span data-ttu-id="b5ae5-182">Az alkalmazás beállítása a kiállító terjesztési hely által kezdeményezett egyszeri Bejelentkezést, a válasz URL-CÍMEN nem kötelező megadni.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-182">To configure the application as IdP-initiated SSO, the Reply URL it’s a required value.</span></span> <span data-ttu-id="b5ae5-183">Egyes alkalmazások azonosító is kötelező megadni.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-183">For some applications, the Identifier is also a required value.</span></span>

10. <span data-ttu-id="b5ae5-184">**Választható lehetőség:** kattintson **megjelenítése speciális URL-beállításainak** Ha meg szeretné tekinteni a nem szükséges értékeket.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-184">**Optional:** click **Show advanced URL settings** if you want to see the non-required values.</span></span>

11. <span data-ttu-id="b5ae5-185">Az a **felhasználói attribútumok**, válassza ki a felhasználók egyedi azonosítója a **felhasználói azonosító** legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-185">In the **User attributes**, select the unique identifier for your users in the **User Identifier** dropdown.</span></span>

12. <span data-ttu-id="b5ae5-186">**Választható lehetőség:** kattintson **nézet és egyéb felhasználói attribútumok szerkesztése** kell küldeni a alkalmazását a SAML-jogkivonat felhasználói bejelentkezés attribútumok szerkesztéséhez.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-186">**Optional:** click **View and edit all other user attributes** to edit the attributes to be sent to the application in the SAML token when user sign in.</span></span>

   <span data-ttu-id="b5ae5-187">Attribútum hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="b5ae5-187">To add an attribute:</span></span>

   1. <span data-ttu-id="b5ae5-188">Kattintson a **Hozzáadás attribútum**.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-188">click **Add attribute**.</span></span> <span data-ttu-id="b5ae5-189">Adja meg a **neve** majd válassza a **érték** a legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-189">Enter the **Name** and the select the **Value** from the dropdown.</span></span>

   2. <span data-ttu-id="b5ae5-190">Kattintson a **mentéséhez.**</span><span class="sxs-lookup"><span data-stu-id="b5ae5-190">Click **Save.**</span></span> <span data-ttu-id="b5ae5-191">Az új attribútumot a táblázatban látható.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-191">You see the new attribute in the table.</span></span>

13. <span data-ttu-id="b5ae5-192">Kattintson a **konfigurálása &lt;alkalmazásnév&gt;**  hozzáférés dokumentációjának egyszeri bejelentkezést az alkalmazás konfigurálásához.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-192">click **Configure &lt;application name&gt;** to access documentation on how to configure single sign-on in the application.</span></span> <span data-ttu-id="b5ae5-193">Is hogy rendelkezik, a metadata URL-címek és az egyszeri bejelentkezés beállítása az alkalmazáshoz szükséges tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-193">Also, you has the metadata URLs and certificate required to setup SSO with the application.</span></span>

14. <span data-ttu-id="b5ae5-194">Kattintson a **mentése** a konfiguráció mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-194">Click **Save** to save the configuration.</span></span>

15. <span data-ttu-id="b5ae5-195">Felhasználók hozzárendelése az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-195">Assign users to the application.</span></span>

### <a name="select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application"></a><span data-ttu-id="b5ae5-196">Válassza ki a felhasználói azonosítóját, és elküldi az alkalmazás felhasználói attribútumok hozzáadása</span><span class="sxs-lookup"><span data-stu-id="b5ae5-196">Select User Identifier and add user attributes to be sent to the application</span></span>

<span data-ttu-id="b5ae5-197">Válassza ki a felhasználói azonosító, vagy adja hozzá a felhasználói attribútumok, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="b5ae5-197">To select the User Identifier or add user attributes, follow the steps below:</span></span>

1.  <span data-ttu-id="b5ae5-198">Nyissa meg a [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**</span><span class="sxs-lookup"><span data-stu-id="b5ae5-198">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="b5ae5-199">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-199">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="b5ae5-200">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-200">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="b5ae5-201">Kattintson a **vállalati alkalmazások** az Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-201">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="b5ae5-202">Kattintson a **összes alkalmazás** az alkalmazások listájának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-202">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="b5ae5-203">Ha nem látja az alkalmazás szeretné itt jelenik meg, akkor használja a **szűrő** vezérlő tetején a **összes alkalmazások listáját** és állítsa be a **megjelenítése** lehetőséggel **összes alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="b5ae5-203">If you do not see the application you want to show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="b5ae5-204">Válassza ki az alkalmazást, az egyszeri bejelentkezés konfigurálta.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-204">Select the application you have configured single sign-on.</span></span>

7.  <span data-ttu-id="b5ae5-205">Ha az alkalmazás betölt, kattintson a **egyszeri bejelentkezés** az alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-205">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="b5ae5-206">Az a **felhasználói attribútumok** területen válassza ki a felhasználók egyedi azonosítója a **felhasználói azonosító** legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-206">Under the **User attributes** section, select the unique identifier for your users in the **User Identifier** dropdown.</span></span> <span data-ttu-id="b5ae5-207">A kiválasztott beállítás a várt érték a felhasználó hitelesítésére az alkalmazásban egyezniük kell.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-207">The selected option needs to match the expected value in the application to authenticate the user.</span></span>

    >[!NOTE]
    ><span data-ttu-id="b5ae5-208">Az Azure AD a NameID attribútum (felhasználói azonosító) formátumát a megadott érték alapján kijelölése, vagy a SAML AuthRequest az alkalmazás által kért formátuma.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-208">Azure AD select the format for the NameID attribute (User Identifier) based on the value selected or the format requested by the application in the SAML AuthRequest.</span></span> <span data-ttu-id="b5ae5-209">További információ látogasson el a [egyszeri bejelentkezés SAML protokoll](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) NameIDPolicy szakaszban.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-209">For more information visit the article [Single Sign-On SAML protocol](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) under the section NameIDPolicy.</span></span>
    >
    >

9.  <span data-ttu-id="b5ae5-210">Felhasználói attribútumok hozzáadásához kattintson **nézet és egyéb felhasználói attribútumok szerkesztése** kell küldeni a alkalmazását a SAML-jogkivonat felhasználói bejelentkezés attribútumok szerkesztéséhez.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-210">To add user attributes, click **View and edit all other user attributes** to edit the attributes to be sent to the application in the SAML token when user sign in.</span></span>

   <span data-ttu-id="b5ae5-211">Attribútum hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="b5ae5-211">To add an attribute:</span></span>

   1. <span data-ttu-id="b5ae5-212">Kattintson a **Hozzáadás attribútum**.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-212">click **Add attribute**.</span></span> <span data-ttu-id="b5ae5-213">Adja meg a **neve** majd válassza a **érték** a legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-213">Enter the **Name** and the select the **Value** from the dropdown.</span></span>

   2. <span data-ttu-id="b5ae5-214">Kattintson a **mentéséhez.**</span><span class="sxs-lookup"><span data-stu-id="b5ae5-214">Click **Save.**</span></span> <span data-ttu-id="b5ae5-215">Az új attribútumot a táblázatban látható.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-215">You see the new attribute in the table.</span></span>

### <a name="download-the-azure-ad-metadata-or-certificate"></a><span data-ttu-id="b5ae5-216">Az Azure AD-metaadatok vagy a tanúsítvány letöltése</span><span class="sxs-lookup"><span data-stu-id="b5ae5-216">Download the Azure AD metadata or certificate</span></span>

<span data-ttu-id="b5ae5-217">Az alkalmazás metaadatainak vagy tanúsítvány letöltéséhez az Azure AD, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="b5ae5-217">To download the application metadata or certificate from Azure AD, follow the steps below:</span></span>

1.  <span data-ttu-id="b5ae5-218">Nyissa meg a [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**</span><span class="sxs-lookup"><span data-stu-id="b5ae5-218">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="b5ae5-219">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-219">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="b5ae5-220">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-220">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="b5ae5-221">Kattintson a **vállalati alkalmazások** az Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-221">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="b5ae5-222">Kattintson a **összes alkalmazás** az alkalmazások listájának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-222">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="b5ae5-223">Ha azt szeretné, hogy itt megjelennek az alkalmazás nem látja, használja a **szűrő** vezérlő tetején a **összes alkalmazások listáját** és állítsa be a **megjelenítése** lehetőséggel **összes alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="b5ae5-223">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="b5ae5-224">Válassza ki az alkalmazást, az egyszeri bejelentkezés konfigurálta.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-224">Select the application you have configured single sign-on.</span></span>

7.  <span data-ttu-id="b5ae5-225">Ha az alkalmazás betölt, kattintson a **egyszeri bejelentkezés** az alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-225">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="b5ae5-226">Ugrás a **SAML-aláíró tanúsítványa** területen, majd kattintson **letöltése** oszlop értékét.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-226">Go to **SAML Signing Certificate** section, then click **Download** column value.</span></span> <span data-ttu-id="b5ae5-227">Attól függően, hogy milyen az alkalmazáshoz az szükséges, az egyszeri bejelentkezés konfigurálása lásd: a metaadatok XML-kód letöltése beállítás, vagy a tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-227">Depending on what the application requires configuring single sign-on, you see either the option to download the Metadata XML or the Certificate.</span></span>

    <span data-ttu-id="b5ae5-228">Az Azure AD nem biztosít a metaadatok beolvasása URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-228">Azure AD doesn’t provide a URL to get the metadata.</span></span> <span data-ttu-id="b5ae5-229">A metaadatok XML-fájlként csak olvasható.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-229">The metadata can only be retrieved as a XML file.</span></span>

## <a name="how-to-configure-federated-single-sign-on-for-a-non-gallery-application"></a><span data-ttu-id="b5ae5-230">Összevont egyszeri bejelentkezés nem galéria alkalmazások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b5ae5-230">How to configure federated single sign-on for a non-gallery application</span></span>

<span data-ttu-id="b5ae5-231">Beállíthat egy nem galéria-alkalmazást, akkor kell rendelkeznie az Azure AD premium és az alkalmazás támogatja az SAML 2.0-s.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-231">To configure a non-gallery application, you need to have Azure AD premium and the application supports SAML 2.0.</span></span> <span data-ttu-id="b5ae5-232">Az Azure AD-verziókkal kapcsolatos további információkért látogasson el a [az Azure AD árképzési](https://azure.microsoft.com/pricing/details/active-directory/).</span><span class="sxs-lookup"><span data-stu-id="b5ae5-232">For more information about Azure AD versions, visit [Azure AD pricing](https://azure.microsoft.com/pricing/details/active-directory/).</span></span>

-   [<span data-ttu-id="b5ae5-233">Az alkalmazás metaadatai értékeinek konfigurálása az Azure AD (bejelentkezési URL-cím, az azonosítója, válasz URL-címe)</span><span class="sxs-lookup"><span data-stu-id="b5ae5-233">Configure the application’s metadata values in Azure AD (Sign on URL, Identifier, Reply URL)</span></span>](#configuring-single-sign-on)

-   [<span data-ttu-id="b5ae5-234">Válassza ki a felhasználói azonosítóját, és elküldi az alkalmazás felhasználói attribútumok hozzáadása</span><span class="sxs-lookup"><span data-stu-id="b5ae5-234">Select User Identifier and add user attributes to be sent to the application</span></span>](#select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application)

-   [<span data-ttu-id="b5ae5-235">Az Azure AD-metaadatok és a tanúsítvány lekérése</span><span class="sxs-lookup"><span data-stu-id="b5ae5-235">Retrieve Azure AD metadata and certificate</span></span>](#download-the-azure-ad-metadata-or-certificate)

-   [<span data-ttu-id="b5ae5-236">Konfigurálja az Azure AD metaadatok értékeket az alkalmazásban (bejelentkezési URL-címet, a kibocsátó, a kijelentkezési URL-cím és a tanúsítvány)</span><span class="sxs-lookup"><span data-stu-id="b5ae5-236">Configure Azure AD metadata values in the application (Sign on URL, Issuer, Logout URL and certificate)</span></span>](#configuring-single-sign-on)

### <a name="configure-the-applications-metadata-values-in-azure-ad-sign-on-url-identifier-reply-url"></a><span data-ttu-id="b5ae5-237">Az alkalmazás metaadatai értékeinek konfigurálása az Azure AD (bejelentkezési URL-cím, az azonosítója, válasz URL-címe)</span><span class="sxs-lookup"><span data-stu-id="b5ae5-237">Configure the application’s metadata values in Azure AD (Sign on URL, Identifier, Reply URL)</span></span>

<span data-ttu-id="b5ae5-238">Egyszeri bejelentkezés egy alkalmazás, amely nincs az Azure AD-katalógus konfigurálásához kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="b5ae5-238">To configure single sign-on for an application that is not in the Azure AD gallery, follow the steps below:</span></span>

1.  <span data-ttu-id="b5ae5-239">Nyissa meg a [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**</span><span class="sxs-lookup"><span data-stu-id="b5ae5-239">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="b5ae5-240">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-240">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="b5ae5-241">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-241">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="b5ae5-242">Kattintson a **vállalati alkalmazások** az Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-242">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="b5ae5-243">Kattintson a **Hozzáadás** gombra a jobb felső sarkában a **vállalati alkalmazások** panel</span><span class="sxs-lookup"><span data-stu-id="b5ae5-243">click the **Add** button at the top-right corner on the **Enterprise Applications** blade</span></span>

6.  <span data-ttu-id="b5ae5-244">Kattintson a **nem-gyűjtemény alkalmazás** a a **saját alkalmazás felvétele** szakasz</span><span class="sxs-lookup"><span data-stu-id="b5ae5-244">click **Non-gallery application** in the **Add your own app** section</span></span>

7.  <span data-ttu-id="b5ae5-245">Adja meg az alkalmazás nevét a **neve** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-245">Enter the name of the application in the **Name** textbox.</span></span>

8.  <span data-ttu-id="b5ae5-246">Kattintson a **Hozzáadás** gombra, vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-246">Click **Add** button, to add the application.</span></span>

9.  <span data-ttu-id="b5ae5-247">Ha az alkalmazás betölt, kattintson a **egyszeri bejelentkezés** az alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-247">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

10. <span data-ttu-id="b5ae5-248">Válassza ki **SAML-alapú bejelentkezés** a a **mód** legördülő lista</span><span class="sxs-lookup"><span data-stu-id="b5ae5-248">Select **SAML-based Sign-on** in the **Mode** dropdown</span></span>

11. <span data-ttu-id="b5ae5-249">Adja meg a szükséges értékeket a **tartomány és az URL-címeket.**</span><span class="sxs-lookup"><span data-stu-id="b5ae5-249">Enter the required values in **Domain and URLs.**</span></span> <span data-ttu-id="b5ae5-250">Ezeket az értékeket az alkalmazás gyártójának szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-250">You should get these values from the application vendor.</span></span>

  1. <span data-ttu-id="b5ae5-251">Az alkalmazás beállítása a kiállító terjesztési hely által kezdeményezett egyszeri Bejelentkezést, írja be a válasz URL-CÍMEN és azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-251">To configure the application as IdP-initiated SSO, enter the Reply URL and the Identifier.</span></span>

  2. <span data-ttu-id="b5ae5-252">**Választható lehetőség:** SP által kezdeményezett egyszeri Bejelentkezést, a bejelentkezési URL-címen, az alkalmazás konfigurálásához szükséges érték.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-252">**Optional:** To configure the application as SP-initiated SSO, the Sign on URL it’s a required value.</span></span>

12. <span data-ttu-id="b5ae5-253">Az a **felhasználói attribútumok**, válassza ki a felhasználók egyedi azonosítója a **felhasználói azonosító** legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-253">In the **User attributes**, select the unique identifier for your users in the **User Identifier** dropdown.</span></span>

13. <span data-ttu-id="b5ae5-254">**Választható lehetőség:** kattintson **nézet és egyéb felhasználói attribútumok szerkesztése** kell küldeni a alkalmazását a SAML-jogkivonat felhasználói bejelentkezés attribútumok szerkesztéséhez.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-254">**Optional:** click **View and edit all other user attributes** to edit the attributes to be sent to the application in the SAML token when user sign in.</span></span>

   <span data-ttu-id="b5ae5-255">Attribútum hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="b5ae5-255">To add an attribute:</span></span>

   1. <span data-ttu-id="b5ae5-256">Kattintson a **Hozzáadás attribútum**.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-256">click **Add attribute**.</span></span> <span data-ttu-id="b5ae5-257">Adja meg a **neve** majd válassza a **érték** a legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-257">Enter the **Name** and the select the **Value** from the dropdown.</span></span>

   2. <span data-ttu-id="b5ae5-258">Kattintson a **mentéséhez.**</span><span class="sxs-lookup"><span data-stu-id="b5ae5-258">Click **Save.**</span></span> <span data-ttu-id="b5ae5-259">Az új attribútumot a táblázatban látható.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-259">You see the new attribute in the table.</span></span>

14. <span data-ttu-id="b5ae5-260">Kattintson a **konfigurálása &lt;alkalmazásnév&gt;**  hozzáférés dokumentációjának egyszeri bejelentkezést az alkalmazás konfigurálásához.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-260">click **Configure &lt;application name&gt;** to access documentation on how to configure single sign-on in the application.</span></span> <span data-ttu-id="b5ae5-261">Is hogy rendelkezik, az Azure AD URL-címek és az alkalmazáshoz szükséges tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-261">Also, you has Azure AD URLs and certificate required for the application.</span></span>

### <a name="select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application"></a><span data-ttu-id="b5ae5-262">Válassza ki a felhasználói azonosítóját, és elküldi az alkalmazás felhasználói attribútumok hozzáadása</span><span class="sxs-lookup"><span data-stu-id="b5ae5-262">Select User Identifier and add user attributes to be sent to the application</span></span>

<span data-ttu-id="b5ae5-263">Válassza ki a felhasználói azonosító, vagy adja hozzá a felhasználói attribútumok, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="b5ae5-263">To select the User Identifier or add user attributes, follow the steps below:</span></span>

1.  <span data-ttu-id="b5ae5-264">Nyissa meg a [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**</span><span class="sxs-lookup"><span data-stu-id="b5ae5-264">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="b5ae5-265">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-265">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="b5ae5-266">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-266">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="b5ae5-267">Kattintson a **vállalati alkalmazások** az Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-267">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="b5ae5-268">Kattintson a **összes alkalmazás** az alkalmazások listájának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-268">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="b5ae5-269">Ha azt szeretné, hogy itt megjelennek az alkalmazás nem látja, használja a **szűrő** vezérlő tetején a **összes alkalmazások listáját** és állítsa be a **megjelenítése** lehetőséggel **összes alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="b5ae5-269">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="b5ae5-270">Válassza ki az alkalmazást, az egyszeri bejelentkezés konfigurálta.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-270">Select the application you have configured single sign-on.</span></span>

7.  <span data-ttu-id="b5ae5-271">Ha az alkalmazás betölt, kattintson a **egyszeri bejelentkezés** az alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-271">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="b5ae5-272">Az a **felhasználói attribútumok** területen válassza ki a felhasználók egyedi azonosítója a **felhasználói azonosító** legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-272">Under the **User attributes** section, select the unique identifier for your users in the **User Identifier** dropdown.</span></span> <span data-ttu-id="b5ae5-273">A kiválasztott beállítás a várt érték a felhasználó hitelesítésére az alkalmazásban egyezniük kell.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-273">The selected option needs to match the expected value in the application to authenticate the user.</span></span>

   >[!NOTE]
   ><span data-ttu-id="b5ae5-274">Az Azure AD a NameID attribútum (felhasználói azonosító) formátumát a megadott érték alapján kijelölése, vagy a SAML AuthRequest az alkalmazás által kért formátuma.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-274">Azure AD select the format for the NameID attribute (User Identifier) based on the value selected or the format requested by the application in the SAML AuthRequest.</span></span> <span data-ttu-id="b5ae5-275">További információ látogasson el a [egyszeri bejelentkezés SAML protokoll](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) NameIDPolicy szakaszban.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-275">For more information visit the article [Single Sign-On SAML protocol](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) under the section NameIDPolicy.</span></span>
   >
   >

9.  <span data-ttu-id="b5ae5-276">Felhasználói attribútumok hozzáadásához kattintson **nézet és egyéb felhasználói attribútumok szerkesztése** kell küldeni a alkalmazását a SAML-jogkivonat felhasználói bejelentkezés attribútumok szerkesztéséhez.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-276">To add user attributes, click **View and edit all other user attributes** to edit the attributes to be sent to the application in the SAML token when user sign in.</span></span>

   <span data-ttu-id="b5ae5-277">Attribútum hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="b5ae5-277">To add an attribute:</span></span>

   <span data-ttu-id="b5ae5-278">1. Kattintson a **Hozzáadás attribútum**.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-278">1.click **Add attribute**.</span></span> <span data-ttu-id="b5ae5-279">Adja meg a **neve** majd válassza a **érték** a legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-279">Enter the **Name** and the select the **Value** from the dropdown.</span></span>

   <span data-ttu-id="b5ae5-280">2 kattintson **mentéséhez.**</span><span class="sxs-lookup"><span data-stu-id="b5ae5-280">2 Click **Save.**</span></span> <span data-ttu-id="b5ae5-281">Az új attribútumot a táblázatban látható.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-281">You see the new attribute in the table.</span></span>

### <a name="download-the-azure-ad-metadata-or-certificate"></a><span data-ttu-id="b5ae5-282">Az Azure AD-metaadatok vagy a tanúsítvány letöltése</span><span class="sxs-lookup"><span data-stu-id="b5ae5-282">Download the Azure AD metadata or certificate</span></span>

<span data-ttu-id="b5ae5-283">Az alkalmazás metaadatainak vagy tanúsítvány letöltéséhez az Azure AD, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="b5ae5-283">To download the application metadata or certificate from Azure AD, follow the steps below:</span></span>

1.  <span data-ttu-id="b5ae5-284">Nyissa meg a [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**</span><span class="sxs-lookup"><span data-stu-id="b5ae5-284">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="b5ae5-285">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-285">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="b5ae5-286">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-286">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="b5ae5-287">Kattintson a **vállalati alkalmazások** az Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-287">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="b5ae5-288">Kattintson a **összes alkalmazás** az alkalmazások listájának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-288">click **All Applications** to view a list of all your applications.</span></span>

   * <span data-ttu-id="b5ae5-289">Ha azt szeretné, hogy itt megjelennek az alkalmazás nem látja, használja a **szűrő** vezérlő tetején a **összes alkalmazások listáját** és állítsa be a **megjelenítése** lehetőséggel **összes alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="b5ae5-289">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="b5ae5-290">Válassza ki az alkalmazást, az egyszeri bejelentkezés konfigurálta.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-290">Select the application you have configured single sign-on.</span></span>

7.  <span data-ttu-id="b5ae5-291">Ha az alkalmazás betölt, kattintson a **egyszeri bejelentkezés** az alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-291">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="b5ae5-292">Ugrás a **SAML-aláíró tanúsítványa** területen, majd kattintson **letöltése** oszlop értékét.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-292">Go to **SAML Signing Certificate** section, then click **Download** column value.</span></span> <span data-ttu-id="b5ae5-293">Attól függően, hogy milyen az alkalmazáshoz az szükséges, az egyszeri bejelentkezés konfigurálása lásd: a metaadatok XML-kód letöltése beállítás, vagy a tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-293">Depending on what the application requires configuring single sign-on, you see either the option to download the Metadata XML or the Certificate.</span></span>

    <span data-ttu-id="b5ae5-294">Az Azure AD nem biztosít a metaadatok beolvasása URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-294">Azure AD doesn’t provide a URL to get the metadata.</span></span> <span data-ttu-id="b5ae5-295">A metaadatok XML-fájlként csak olvasható.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-295">The metadata can only be retrieved as a XML file.</span></span>

## <a name="how-to-configure-password-single-sign-on-for-an-azure-ad-gallery-application"></a><span data-ttu-id="b5ae5-296">Jelszó egyszeri bejelentkezés az Azure AD gyűjtemény alkalmazás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b5ae5-296">How to configure password single sign-on for an Azure AD gallery application</span></span>

<span data-ttu-id="b5ae5-297">Egy alkalmazás kell az Azure AD-galériából konfigurálása:</span><span class="sxs-lookup"><span data-stu-id="b5ae5-297">To configure an application from the Azure AD gallery you need to:</span></span>

-   [<span data-ttu-id="b5ae5-298">Alkalmazás hozzáadása az Azure AD-galériából</span><span class="sxs-lookup"><span data-stu-id="b5ae5-298">Add an application from the Azure AD gallery</span></span>](#add-an-application)

-   [<span data-ttu-id="b5ae5-299">Állítsa be az alkalmazását jelszó egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-299">Configure the application for password single sign-on</span></span>](#configure-the-application)

### <a name="add-an-application-from-the-azure-ad-gallery"></a><span data-ttu-id="b5ae5-300">Alkalmazás hozzáadása az Azure AD-galériából</span><span class="sxs-lookup"><span data-stu-id="b5ae5-300">Add an application from the Azure AD gallery</span></span>

<span data-ttu-id="b5ae5-301">Hozzáadhat egy alkalmazást az Azure AD-gyűjteményből, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="b5ae5-301">To add an application from the Azure AD Gallery, follow the steps below:</span></span>

1.  <span data-ttu-id="b5ae5-302">Nyissa meg a [Azure Portal](https://portal.azure.com) , és jelentkezzen be egy **globális rendszergazda** vagy **társadminisztrátornak**</span><span class="sxs-lookup"><span data-stu-id="b5ae5-302">Open the [Azure Portal](https://portal.azure.com) and sign in as a **Global Administrator** or **Co-admin**</span></span>

2.  <span data-ttu-id="b5ae5-303">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-303">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="b5ae5-304">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-304">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="b5ae5-305">Kattintson a **vállalati alkalmazások** az Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-305">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="b5ae5-306">Kattintson a **Hozzáadás** gombra a jobb felső sarkában a **vállalati alkalmazások** panel</span><span class="sxs-lookup"><span data-stu-id="b5ae5-306">click the **Add** button at the top-right corner on the **Enterprise Applications** blade</span></span>

6.  <span data-ttu-id="b5ae5-307">Az a **adjon meg egy nevet** a szövegmező a **hozzáadása a gyűjteményből** területen írja be az alkalmazás nevét</span><span class="sxs-lookup"><span data-stu-id="b5ae5-307">In the **Enter a name** textbox from the **Add from the gallery** section, type the name of the application</span></span>

7.  <span data-ttu-id="b5ae5-308">Válassza ki az alkalmazást szeretne konfigurálni az egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-308">Select the application you want to configure for single sign-on</span></span>

8.  <span data-ttu-id="b5ae5-309">Ad hozzá az alkalmazást, mielőtt a nevét módosíthatja a **neve** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-309">Before adding the application, you can change its name from the **Name** textbox.</span></span>

9.  <span data-ttu-id="b5ae5-310">Kattintson a **Hozzáadás** gombra, vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-310">Click **Add** button, to add the application.</span></span>

<span data-ttu-id="b5ae5-311">Egy rövid időszak után el az alkalmazás konfigurációs panelen láthatók.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-311">After a short period, you be able to see the application’s configuration blade.</span></span>

### <a name="configure-the-application-for-password-single-sign-on"></a><span data-ttu-id="b5ae5-312">Állítsa be az alkalmazását jelszó egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-312">Configure the application for password single sign-on</span></span>

<span data-ttu-id="b5ae5-313">Egyszeri bejelentkezés egy alkalmazás konfigurálásához kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="b5ae5-313">To configure single sign-on for an application, follow the steps below:</span></span>

1.  <span data-ttu-id="b5ae5-314">Nyissa meg a [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**</span><span class="sxs-lookup"><span data-stu-id="b5ae5-314">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="b5ae5-315">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-315">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="b5ae5-316">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-316">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="b5ae5-317">Kattintson a **vállalati alkalmazások** az Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-317">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="b5ae5-318">Kattintson a **összes alkalmazás** az alkalmazások listájának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-318">click **All Applications** to view a list of all your applications.</span></span>

 * <span data-ttu-id="b5ae5-319">Ha azt szeretné, hogy itt megjelennek az alkalmazás nem látja, használja a **szűrő** vezérlő tetején a **összes alkalmazások listáját** és állítsa be a **megjelenítése** lehetőséggel **összes alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="b5ae5-319">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="b5ae5-320">Válassza ki az egyszeri bejelentkezés konfigurálni kívánt alkalmazást</span><span class="sxs-lookup"><span data-stu-id="b5ae5-320">Select the application you want to configure single sign-on</span></span>

7.  <span data-ttu-id="b5ae5-321">Ha az alkalmazás betölt, kattintson a **egyszeri bejelentkezés** az alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-321">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="b5ae5-322">Válassza ki a módot **jelszóalapú bejelentkezés.**</span><span class="sxs-lookup"><span data-stu-id="b5ae5-322">Select the mode **Password-based Sign-on.**</span></span>

9.  <span data-ttu-id="b5ae5-323">Felhasználók hozzárendelése az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-323">Assign users to the application.</span></span>

10. <span data-ttu-id="b5ae5-324">Emellett is megadhatja a felhasználó nevében hitelesítő adatok a felhasználók a sorok kijelöléséhez és parancsával **frissítéséhez szükséges hitelesítő adatokat** , majd gépelje be a felhasználónevet és jelszót a felhasználók nevében.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-324">Additionally, you can also provide credentials on behalf of the user by selecting the rows of the users and clicking on **Update Credentials** and entering the username and password on behalf of the users.</span></span> <span data-ttu-id="b5ae5-325">Ellenkező esetben megkérdezi a felhasználókat a hitelesítő adatok magukat az indítás után.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-325">Otherwise, users be prompted to enter the credentials themselves upon launch.</span></span>

## <a name="how-to-configure-password-single-sign-on-for-a-non-gallery-application"></a><span data-ttu-id="b5ae5-326">Jelszó egyszeri bejelentkezés nem galéria alkalmazások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b5ae5-326">How to configure password single sign-on for a non-gallery application</span></span>

<span data-ttu-id="b5ae5-327">Egy alkalmazás kell az Azure AD-galériából konfigurálása:</span><span class="sxs-lookup"><span data-stu-id="b5ae5-327">To configure an application from the Azure AD gallery you need to:</span></span>

-   [<span data-ttu-id="b5ae5-328">Nem galéria alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="b5ae5-328">Add a non-gallery application</span></span>](#add-a-non-gallery-application)

-   [<span data-ttu-id="b5ae5-329">Állítsa be az alkalmazását jelszó egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-329">Configure the application for password single sign-on</span></span>](#configure-the-application-for-password-single-sign-on)

### <a name="add-a-non-gallery-application"></a><span data-ttu-id="b5ae5-330">Nem galéria alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="b5ae5-330">Add a non-gallery application</span></span>

<span data-ttu-id="b5ae5-331">Hozzáadhat egy alkalmazást az Azure AD-gyűjteményből, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="b5ae5-331">To add an application from the Azure AD Gallery, follow the steps below:</span></span>

1.  <span data-ttu-id="b5ae5-332">Nyissa meg a [Azure Portal](https://portal.azure.com) , és jelentkezzen be egy **globális rendszergazda** vagy **társadminisztrátornak**</span><span class="sxs-lookup"><span data-stu-id="b5ae5-332">Open the [Azure Portal](https://portal.azure.com) and sign in as a **Global Administrator** or **Co-admin**</span></span>

2.  <span data-ttu-id="b5ae5-333">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-333">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="b5ae5-334">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-334">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="b5ae5-335">Kattintson a **vállalati alkalmazások** az Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-335">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="b5ae5-336">Kattintson a **Hozzáadás** gombra a jobb felső sarkában a **vállalati alkalmazások** panel</span><span class="sxs-lookup"><span data-stu-id="b5ae5-336">click the **Add** button at the top-right corner on the **Enterprise Applications** blade</span></span>

6.  <span data-ttu-id="b5ae5-337">Kattintson a **nem-gyűjtemény alkalmazás.**</span><span class="sxs-lookup"><span data-stu-id="b5ae5-337">click **Non-gallery application.**</span></span>

7.  <span data-ttu-id="b5ae5-338">Adja meg az alkalmazás nevét a **neve** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-338">Enter the name of your application in the **Name** textbox.</span></span> <span data-ttu-id="b5ae5-339">Válassza ki **hozzáadása.**</span><span class="sxs-lookup"><span data-stu-id="b5ae5-339">Select **Add.**</span></span>

<span data-ttu-id="b5ae5-340">Egy rövid időszak után el az alkalmazás konfigurációs panelen láthatók.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-340">After a short period, you be able to see the application’s configuration blade.</span></span>

### <a name="configure-the-application-for-password-single-sign-on"></a><span data-ttu-id="b5ae5-341">Állítsa be az alkalmazását jelszó egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-341">Configure the application for password single sign-on</span></span>

<span data-ttu-id="b5ae5-342">Egyszeri bejelentkezés egy alkalmazás konfigurálásához kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="b5ae5-342">To configure single sign-on for an application, follow the steps below:</span></span>

1.  <span data-ttu-id="b5ae5-343">Nyissa meg a [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**</span><span class="sxs-lookup"><span data-stu-id="b5ae5-343">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="b5ae5-344">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-344">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="b5ae5-345">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-345">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="b5ae5-346">Kattintson a **vállalati alkalmazások** az Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-346">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="b5ae5-347">Kattintson a **összes alkalmazás** az alkalmazások listájának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-347">click **All Applications** to view a list of all your applications.</span></span>

 * <span data-ttu-id="b5ae5-348">Ha azt szeretné, hogy itt megjelennek az alkalmazás nem látja, használja a **szűrő** vezérlő tetején a **összes alkalmazások listáját** és állítsa be a **megjelenítése** lehetőséggel **összes alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="b5ae5-348">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="b5ae5-349">Válassza ki az egyszeri bejelentkezés konfigurálni kívánt alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-349">Select the application you want to configure single sign-on.</span></span>

7.  <span data-ttu-id="b5ae5-350">Ha az alkalmazás betölt, kattintson a **egyszeri bejelentkezés** az alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-350">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="b5ae5-351">Válassza ki a módot **jelszóalapú bejelentkezés.**</span><span class="sxs-lookup"><span data-stu-id="b5ae5-351">Select the mode **Password-based Sign-on.**</span></span>

9.  <span data-ttu-id="b5ae5-352">Adja meg a **bejelentkezési URL-cím**.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-352">Enter the **Sign-on URL**.</span></span> <span data-ttu-id="b5ae5-353">Ez az URL-CÍMÉT ahol adja meg a felhasználók a felhasználónevével és jelszavával bejelentkezni.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-353">This is the URL where users enter their username and password to sign in to.</span></span> <span data-ttu-id="b5ae5-354">Győződjön meg arról, a mezők bejelentkezési URL-címen láthatók.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-354">Ensure the sign in fields are visible at the URL.</span></span>

10. <span data-ttu-id="b5ae5-355">Felhasználók hozzárendelése az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-355">Assign users to the application.</span></span>

11. <span data-ttu-id="b5ae5-356">Emellett is megadhatja a felhasználó nevében hitelesítő adatok a felhasználók a sorok kijelöléséhez és parancsával **frissítéséhez szükséges hitelesítő adatokat** , majd gépelje be a felhasználónevet és jelszót a felhasználók nevében.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-356">Additionally, you can also provide credentials on behalf of the user by selecting the rows of the users and clicking on **Update Credentials** and entering the username and password on behalf of the users.</span></span> <span data-ttu-id="b5ae5-357">Ellenkező esetben megkérdezi a felhasználókat a hitelesítő adatok magukat az indítás után.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-357">Otherwise, users be prompted to enter the credentials themselves upon launch.</span></span>

## <a name="how-to-assign-a-user-to-an-application-directly"></a><span data-ttu-id="b5ae5-358">A felhasználó közvetlenül egy alkalmazás hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="b5ae5-358">How to assign a user to an application directly</span></span>

<span data-ttu-id="b5ae5-359">Hozzárendelése egy vagy több felhasználó alkalmazás közvetlenül, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="b5ae5-359">To assign one or more users to an application directly, follow the steps below:</span></span>

1.  <span data-ttu-id="b5ae5-360">Nyissa meg a [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazdája.**</span><span class="sxs-lookup"><span data-stu-id="b5ae5-360">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="b5ae5-361">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-361">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="b5ae5-362">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-362">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="b5ae5-363">Kattintson a **vállalati alkalmazások** az Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-363">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="b5ae5-364">Kattintson a **összes alkalmazás** az alkalmazások listájának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-364">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="b5ae5-365">Ha azt szeretné, hogy itt megjelennek az alkalmazás nem látja, használja a **szűrő** vezérlő tetején a **összes alkalmazások listáját** és állítsa be a **megjelenítése** lehetőséggel **összes alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="b5ae5-365">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="b5ae5-366">Válassza ki szeretné osztani a felhasználót, hogy a listában az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-366">Select the application you want to assign a user to from the list.</span></span>

7.  <span data-ttu-id="b5ae5-367">Ha az alkalmazás betölt, kattintson **felhasználók és csoportok** az alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-367">Once the application loads, click **Users and Groups** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="b5ae5-368">Kattintson a **Hozzáadás** gombra kattint, a a **felhasználók és csoportok** nyissa meg a listában a **hozzáadása hozzárendelés** panelen.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-368">Click the **Add** button on top of the **Users and Groups** list to open the **Add Assignment** blade.</span></span>

9.  <span data-ttu-id="b5ae5-369">Kattintson a **felhasználók és csoportok** a választó a **hozzáadása hozzárendelés** panelen.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-369">click the **Users and groups** selector from the **Add Assignment** blade.</span></span>

10. <span data-ttu-id="b5ae5-370">Írja be a **teljes név** vagy **e-mail cím** érdekli hozzárendelése a felhasználó a **Keresés név vagy e-mail cím alapján** keresőmezőbe.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-370">Type in the **full name** or **email address** of the user you are interested in assigning into the **Search by name or email address** search box.</span></span>

11. <span data-ttu-id="b5ae5-371">Vigye a **felhasználói** a listában, hogy láthatóvá váljon a **jelölőnégyzet**.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-371">Hover over the **user** in the list to reveal a **checkbox**.</span></span> <span data-ttu-id="b5ae5-372">A felhasználói profil fénykép vagy adja hozzá a felhasználót emblémát jelölőnégyzetét, kattintson a **kijelölt** listája.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-372">Click the checkbox next to the user’s profile photo or logo to add your user to the **Selected** list.</span></span>

12. <span data-ttu-id="b5ae5-373">**Választható lehetőség:** Ha azt szeretné, hogy **egynél több felhasználó hozzáadása**, egy másik típus **teljes név** vagy **e-mail cím** be a **Keresés név vagy e-mail cím alapján** mező, és a jelölőnégyzet bejelölésével adja hozzá a felhasználót, hogy a **kijelölt** listája.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-373">**Optional:** If you would like to **add more than one user**, type in another **full name** or **email address** into the **Search by name or email address** search box, and click the checkbox to add this user to the **Selected** list.</span></span>

13. <span data-ttu-id="b5ae5-374">Ha elkészült, válassza a felhasználók, kattintson a **válasszon** gombra kattintva vegye fel a listára a felhasználók és csoportok hozzá kell rendelni az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-374">When you are finished selecting users, click the **Select** button to add them to the list of users and groups to be assigned to the application.</span></span>

14. <span data-ttu-id="b5ae5-375">**Választható lehetőség:** kattintson a **Szerepkörválasztás** a választó a **hozzáadása hozzárendelés** hozzárendelése a kiválasztott felhasználói szerepkör kiválasztása panel.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-375">**Optional:** click the **Select Role** selector in the **Add Assignment** blade to select a role to assign to the users you have selected.</span></span>

15. <span data-ttu-id="b5ae5-376">Kattintson a **hozzárendelése** gombra kattintva a kijelölt felhasználók az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-376">Click the **Assign** button to assign the application to the selected users.</span></span>

<span data-ttu-id="b5ae5-377">Rövid ideig a kijelölt felhasználók tudják elindítani ezeket az alkalmazásokat a hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-377">After a short period, the users you have selected be able to launch these applications in the Access Panel.</span></span>

## <a name="if-these-troubleshooting-steps-do-not-the-resolve-the-issue"></a><span data-ttu-id="b5ae5-378">Ha ezek a hibaelhárítási lépéseket nem a hárítsa el a problémát</span><span class="sxs-lookup"><span data-stu-id="b5ae5-378">If these troubleshooting steps do not the resolve the issue</span></span>

<span data-ttu-id="b5ae5-379">támogatási jegy megnyitása a következő információkat, ha rendelkezésre áll:</span><span class="sxs-lookup"><span data-stu-id="b5ae5-379">open a support ticket with the following information if available:</span></span>

-   <span data-ttu-id="b5ae5-380">Megfelelési hiba azonosítója</span><span class="sxs-lookup"><span data-stu-id="b5ae5-380">Correlation error ID</span></span>

-   <span data-ttu-id="b5ae5-381">Egyszerű felhasználónév (felhasználó e-mail címe)</span><span class="sxs-lookup"><span data-stu-id="b5ae5-381">UPN (user email address)</span></span>

-   <span data-ttu-id="b5ae5-382">A TenantID</span><span class="sxs-lookup"><span data-stu-id="b5ae5-382">TenantID</span></span>

-   <span data-ttu-id="b5ae5-383">Böngésző típusa</span><span class="sxs-lookup"><span data-stu-id="b5ae5-383">Browser type</span></span>

-   <span data-ttu-id="b5ae5-384">Időzóna és idő/időkeretre során hiba történik.</span><span class="sxs-lookup"><span data-stu-id="b5ae5-384">Time zone and time/timeframe during error occurs</span></span>

-   <span data-ttu-id="b5ae5-385">Fiddler nyomkövetések</span><span class="sxs-lookup"><span data-stu-id="b5ae5-385">Fiddler traces</span></span>

## <a name="next-steps"></a><span data-ttu-id="b5ae5-386">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b5ae5-386">Next steps</span></span>
[<span data-ttu-id="b5ae5-387">Adja meg az egyszeri bejelentkezés az alkalmazásokba a Proxy</span><span class="sxs-lookup"><span data-stu-id="b5ae5-387">Provide single sign-on to your apps with Application Proxy</span></span>](active-directory-application-proxy-sso-using-kcd.md)

