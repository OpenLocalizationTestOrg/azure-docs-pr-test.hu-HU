---
title: "bejelentkezés hello hozzáférési panel tooan alkalmazás aaaProblems |} Microsoft Docs"
description: "Hogyan tootroubleshoot problémák az alkalmazáshoz való hozzáférés hello myapps.microsoft.com a Microsoft Azure AD hozzáférési Panel"
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
ms.openlocfilehash: 346c4da06416bb9b330bdd5b1201253af19ba58b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="problems-signing-in-tooan-application-from-hello-access-panel"></a><span data-ttu-id="a96c6-103">A hozzáférési panel hello tooan alkalmazásban problémák</span><span class="sxs-lookup"><span data-stu-id="a96c6-103">Problems signing in tooan application from hello access panel</span></span>

<span data-ttu-id="a96c6-104">hello hozzáférési Panel egy webes portál, amely lehetővé teszi a felhasználó munkahelyi vagy iskolai fiókkal az Azure Active Directory (Azure AD) tooview és kezdő felhőalapú alkalmazások, hogy hello Azure AD-rendszergazda rendelkezik hozzáféréssel őket.</span><span class="sxs-lookup"><span data-stu-id="a96c6-104">hello Access Panel is a web-based portal which enables a user with a work or school account in Azure Active Directory (Azure AD) tooview and start cloud-based applications that hello Azure AD administrator has granted them access to.</span></span> 

<span data-ttu-id="a96c6-105">Ezeket az alkalmazásokat úgy vannak konfigurálva, hello Azure AD portálon hello felhasználó nevében.</span><span class="sxs-lookup"><span data-stu-id="a96c6-105">These applications are configured on behalf of hello user in hello Azure AD portal.</span></span> <span data-ttu-id="a96c6-106">hello alkalmazás megfelelően kell konfigurálni, és a hozzárendelt toohello felhasználó vagy csoport hello felhasználó tagja a hozzáférési Panel hello toosee hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="a96c6-106">hello application must be configured properly and assigned toohello user or a group hello user is a member of toosee hello application in hello Access Panel.</span></span>

<span data-ttu-id="a96c6-107">a következő kategóriák hello esik azért jelent meg a felhasználó alkalmazások hello típusa:</span><span class="sxs-lookup"><span data-stu-id="a96c6-107">hello type of apps a user may be seeing fall in hello following categories:</span></span>

-   <span data-ttu-id="a96c6-108">Office 365-alkalmazások</span><span class="sxs-lookup"><span data-stu-id="a96c6-108">Office 365 Applications</span></span>

-   <span data-ttu-id="a96c6-109">Összevonási-alapú egyszeri bejelentkezési modellel konfigurált a Microsoft és külső alkalmazások</span><span class="sxs-lookup"><span data-stu-id="a96c6-109">Microsoft and third-party applications configured with federation-based SSO</span></span>

-   <span data-ttu-id="a96c6-110">Jelszó-alapú egyszeri bejelentkezés alkalmazásokhoz</span><span class="sxs-lookup"><span data-stu-id="a96c6-110">Password-based SSO applications</span></span>

-   <span data-ttu-id="a96c6-111">A már meglévő SSO megoldásaival alkalmazások</span><span class="sxs-lookup"><span data-stu-id="a96c6-111">Applications with existing SSO solutions</span></span>

## <a name="general-issues-toocheck-first"></a><span data-ttu-id="a96c6-112">Általános problémák első toocheck</span><span class="sxs-lookup"><span data-stu-id="a96c6-112">General issues toocheck first</span></span>

-   <span data-ttu-id="a96c6-113">Győződjön meg arról, hogy a használatával egy **böngésző** , amely megfelel a hozzáférési Panel hello hello minimális követelményeinek.</span><span class="sxs-lookup"><span data-stu-id="a96c6-113">Make sure your using a **browser** that meets hello minimum requirements for hello Access Panel.</span></span>

-   <span data-ttu-id="a96c6-114">Ellenőrizze, hogy hello böngésző hello alkalmazás tooits hello URL-CÍMÉT hozzáadta **megbízható helyek**.</span><span class="sxs-lookup"><span data-stu-id="a96c6-114">Make sure hello user’s browser has added hello URL of hello application tooits **trusted sites**.</span></span>

-   <span data-ttu-id="a96c6-115">Ellenőrizze, hogy toocheck hello alkalmazás **konfigurált** megfelelően.</span><span class="sxs-lookup"><span data-stu-id="a96c6-115">Make sure toocheck hello application is **configured** correctly.</span></span>

-   <span data-ttu-id="a96c6-116">Ellenőrizze, hogy a hello felhasználói fiók **engedélyezett** a indított bejelentkezések.</span><span class="sxs-lookup"><span data-stu-id="a96c6-116">Make sure hello user’s account is **enabled** for sign ins.</span></span>

-   <span data-ttu-id="a96c6-117">Ellenőrizze, hogy a hello felhasználói fiók **nincs zárolva.**</span><span class="sxs-lookup"><span data-stu-id="a96c6-117">Make sure hello user’s account is **not locked out.**</span></span>

-   <span data-ttu-id="a96c6-118">Győződjön meg arról, hogy hello felhasználói **nem lejárt vagy elfelejtett jelszó.**</span><span class="sxs-lookup"><span data-stu-id="a96c6-118">Make sure hello user’s **password is not expired or forgotten.**</span></span>

-   <span data-ttu-id="a96c6-119">Győződjön meg arról, hogy **multi-factor Authentication** nem blokkolja a hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="a96c6-119">Make sure **Multi-Factor Authentication** is not blocking user access.</span></span>

-   <span data-ttu-id="a96c6-120">Győződjön meg arról, hogy egy **feltételes hozzáférési házirend** vagy **Identity Protection** házirend nem blokkolja a hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="a96c6-120">Make sure a **Conditional Access policy** or **Identity Protection** policy is not blocking user access.</span></span>

-   <span data-ttu-id="a96c6-121">Győződjön meg arról, hogy a felhasználó **hitelesítési kapcsolattartási adatai** működik-e toodate tooallow többtényezős hitelesítést vagy feltételes hozzáférési házirendek toobe lépnek érvénybe.</span><span class="sxs-lookup"><span data-stu-id="a96c6-121">Make sure that a user’s **authentication contact info** is up toodate tooallow Multi-Factor Authentication or Conditional Access policies toobe enforced.</span></span>

-   <span data-ttu-id="a96c6-122">Győződjön meg arról, hogy tooalso próbálkozzon a böngésző cookie-k törölje, majd próbálkozzon újra a toosign.</span><span class="sxs-lookup"><span data-stu-id="a96c6-122">Make sure tooalso try clearing your browser’s cookies and trying toosign in again.</span></span>

## <a name="meeting-browser-requirements-for-hello-access-panel"></a><span data-ttu-id="a96c6-123">A hozzáférési Panel hello értekezlet böngészőre vonatkozó követelményei</span><span class="sxs-lookup"><span data-stu-id="a96c6-123">Meeting browser requirements for hello Access Panel</span></span>

<span data-ttu-id="a96c6-124">hello hozzáférési Panel igényel, amely támogatja a JavaScript egy böngészőt, és CSS engedélyezte.</span><span class="sxs-lookup"><span data-stu-id="a96c6-124">hello Access Panel requires a browser that supports JavaScript and has CSS enabled.</span></span> <span data-ttu-id="a96c6-125">toouse jelszó-alapú egyszeri bejelentkezést (SSO) a hozzáférési Panel, a hozzáférési Panel bővítmény hello hello telepíteni kell hello felhasználó böngészőjében.</span><span class="sxs-lookup"><span data-stu-id="a96c6-125">toouse password-based single sign-on (SSO) in hello Access Panel, hello Access Panel extension must be installed in hello user’s browser.</span></span> <span data-ttu-id="a96c6-126">A bővítmény le automatikusan, ha a felhasználó megadja egy alkalmazás, amely jelszóalapú SSO van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="a96c6-126">This extension is downloaded automatically when a user selects an application that is configured for password-based SSO.</span></span>

<span data-ttu-id="a96c6-127">Az egyszeri bejelentkezés jelszóalapú hello végfelhasználó böngészőkkel lehet:</span><span class="sxs-lookup"><span data-stu-id="a96c6-127">For password-based SSO, hello end user’s browsers can be:</span></span>

-   <span data-ttu-id="a96c6-128">Internet Explorer 8, 9, 10, 11 – a Windows 7 vagy újabb</span><span class="sxs-lookup"><span data-stu-id="a96c6-128">Internet Explorer 8, 9, 10, 11 -- on Windows 7 or later</span></span>

-   <span data-ttu-id="a96c6-129">Peremhálózati Windows 10 évforduló Edition vagy újabb</span><span class="sxs-lookup"><span data-stu-id="a96c6-129">Edge on Windows 10 Anniversary Edition or later</span></span>

-   <span data-ttu-id="a96c6-130">Chrome – A Windows 7 vagy újabb, és MacOS X rendszeren vagy újabb</span><span class="sxs-lookup"><span data-stu-id="a96c6-130">Chrome -- on Windows 7 or later, and on MacOS X or later</span></span>

-   <span data-ttu-id="a96c6-131">Firefox 26.0 vagy újabb – a Windows XP SP2 vagy újabb, és a Mac OS X 10,6 vagy újabb verzió</span><span class="sxs-lookup"><span data-stu-id="a96c6-131">Firefox 26.0 or later -- on Windows XP SP2 or later, and on Mac OS X 10.6 or later</span></span>

## <a name="how-tooinstall-hello-access-panel-browser-extension"></a><span data-ttu-id="a96c6-132">Hogyan tooinstall hello hozzáférési Panel bővítmény</span><span class="sxs-lookup"><span data-stu-id="a96c6-132">How tooinstall hello Access Panel Browser extension</span></span>

<span data-ttu-id="a96c6-133">tooinstall hello hozzáférési Panel bővítmény, kövesse az alábbi hello lépéseket:</span><span class="sxs-lookup"><span data-stu-id="a96c6-133">tooinstall hello Access Panel Browser extension, follow hello steps below:</span></span>

1.  <span data-ttu-id="a96c6-134">Nyissa meg hello [hozzáférési Panel](https://myapps.microsoft.com) valamelyik hello támogatott böngészők és való bejelentkezést egy **felhasználói** az Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="a96c6-134">Open hello [Access Panel](https://myapps.microsoft.com) in one of hello supported browsers and sign in as a **user** in your Azure AD.</span></span>

2.  <span data-ttu-id="a96c6-135">Kattintson egy **jelszó-SSO alkalmazás** a hozzáférési Panel hello.</span><span class="sxs-lookup"><span data-stu-id="a96c6-135">Click a **password-SSO application** in hello Access Panel.</span></span>

3.  <span data-ttu-id="a96c6-136">Hello Rákérdezés azzal a kérdéssel tooinstall hello szoftver, válassza ki **telepítés**.</span><span class="sxs-lookup"><span data-stu-id="a96c6-136">In hello prompt asking tooinstall hello software, select **Install Now**.</span></span>

4.  <span data-ttu-id="a96c6-137">A böngésző alapján kell irányított toohello letöltési hivatkozását.</span><span class="sxs-lookup"><span data-stu-id="a96c6-137">Based on your browser you be directed toohello download link.</span></span> <span data-ttu-id="a96c6-138">**Adja hozzá** hello bővítmény tooyour böngésző.</span><span class="sxs-lookup"><span data-stu-id="a96c6-138">**Add** hello extension tooyour browser.</span></span>

5.  <span data-ttu-id="a96c6-139">Ha a böngésző kéri, válassza ki a tooeither **engedélyezése** vagy **engedélyezése** hello bővítmény.</span><span class="sxs-lookup"><span data-stu-id="a96c6-139">If your browser asks, select tooeither **Enable** or **Allow** hello extension.</span></span>

6.  <span data-ttu-id="a96c6-140">Korábban telepítve, **indítsa újra a** a böngésző-munkamenetet.</span><span class="sxs-lookup"><span data-stu-id="a96c6-140">Once installed, **restart** your browser session.</span></span>

7.  <span data-ttu-id="a96c6-141">Jelentkezzen be a hozzáférési Panel hello, és tekintse meg, ha **indítása** a jelszó-egyszeri bejelentkezés alkalmazásokhoz</span><span class="sxs-lookup"><span data-stu-id="a96c6-141">Sign in into hello Access Panel and see if you can **launch** your password-SSO applications</span></span>

<span data-ttu-id="a96c6-142">Az alábbi hello közvetlen hivatkozások Chrome és a peremhálózati is letöltheti hello bővítmény:</span><span class="sxs-lookup"><span data-stu-id="a96c6-142">You may also download hello extension for Chrome and Edge from hello direct links below:</span></span>

-   [<span data-ttu-id="a96c6-143">Chrome hozzáférési Panel bővítmény</span><span class="sxs-lookup"><span data-stu-id="a96c6-143">Chrome Access Panel Extension</span></span>](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

-   [<span data-ttu-id="a96c6-144">Peremhálózati hozzáférési Panel bővítmény</span><span class="sxs-lookup"><span data-stu-id="a96c6-144">Edge Access Panel Extension</span></span>](https://www.microsoft.com/store/apps/9pc9sckkzk84)

## <a name="how-tooconfigure-federated-single-sign-on-for-an-azure-ad-gallery-application"></a><span data-ttu-id="a96c6-145">Hogyan tooconfigure összevont egyszeri bejelentkezés az Azure AD gyűjtemény alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="a96c6-145">How tooconfigure federated single sign-on for an Azure AD gallery application</span></span>

<span data-ttu-id="a96c6-146">Hello Azure AD-dokumentumtárban vállalati egyszeri bejelentkezésre alkalmas engedélyezve van az összes alkalmazásnak érhető el részletes oktatóanyaga.</span><span class="sxs-lookup"><span data-stu-id="a96c6-146">All application in hello Azure AD gallery enabled with Enterprise Single Sign-On capability has a step-by-step tutorial available.</span></span> <span data-ttu-id="a96c6-147">Van-e hozzáférési hello [kapcsolatos bemutatók felsorolása toointegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/) kapcsolatos részletek részletes útmutatás.</span><span class="sxs-lookup"><span data-stu-id="a96c6-147">You can access hello [List of tutorials on how toointegrate SaaS apps with Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/) for a detail step-by-step guidance.</span></span>

<span data-ttu-id="a96c6-148">egy alkalmazás hello Azure AD-galériából tooconfigure kell:</span><span class="sxs-lookup"><span data-stu-id="a96c6-148">tooconfigure an application from hello Azure AD gallery you need to:</span></span>

-   [<span data-ttu-id="a96c6-149">Alkalmazás hozzáadása hello Azure AD-galériából</span><span class="sxs-lookup"><span data-stu-id="a96c6-149">Add an application from hello Azure AD gallery</span></span>](#add-an-application)

-   [<span data-ttu-id="a96c6-150">Hello alkalmazás metaadatainak értékek konfigurálása az Azure AD (bejelentkezési URL-cím, az azonosítója, válasz URL-címe)</span><span class="sxs-lookup"><span data-stu-id="a96c6-150">Configure hello application’s metadata values in Azure AD (Sign on URL, Identifier, Reply URL)</span></span>](#configure-single-sign-on-for-an-application-from-the-azure-ad-gallery)

-   [<span data-ttu-id="a96c6-151">Válassza ki a felhasználói azonosítót és a felhasználói attribútumok küldött toobe toohello alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="a96c6-151">Select User Identifier and add user attributes toobe sent toohello application</span></span>](#select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application)

-   [<span data-ttu-id="a96c6-152">Az Azure AD-metaadatok és a tanúsítvány lekérése</span><span class="sxs-lookup"><span data-stu-id="a96c6-152">Retrieve Azure AD metadata and certificate</span></span>](#download-the-azure-ad-metadata-or-certificate)

-   [<span data-ttu-id="a96c6-153">Konfigurálja az Azure AD metaadatok értékeket hello alkalmazásban (bejelentkezési URL-címet, a kibocsátó, a kijelentkezési URL-cím és a tanúsítvány)</span><span class="sxs-lookup"><span data-stu-id="a96c6-153">Configure Azure AD metadata values in hello application (Sign on URL, Issuer, Logout URL and certificate)</span></span>](#configure-single-sign-on-for-an-application-from-the-azure-ad-gallery)

-   [<span data-ttu-id="a96c6-154">Felhasználók hozzárendelése toohello alkalmazás</span><span class="sxs-lookup"><span data-stu-id="a96c6-154">Assign users toohello application</span></span>](#assign-users-to-the-application)

### <a name="add-an-application-from-hello-azure-ad-gallery"></a><span data-ttu-id="a96c6-155">Alkalmazás hozzáadása hello Azure AD-galériából</span><span class="sxs-lookup"><span data-stu-id="a96c6-155">Add an application from hello Azure AD gallery</span></span>

<span data-ttu-id="a96c6-156">az alkalmazás a hello Azure AD-katalógusában, tooadd kövesse hello alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="a96c6-156">tooadd an application from hello Azure AD Gallery, follow hello steps below:</span></span>

1.  <span data-ttu-id="a96c6-157">Nyissa meg hello [Azure Portal](https://portal.azure.com) , és jelentkezzen be a egy **globális rendszergazda** vagy **társadminisztrátornak**</span><span class="sxs-lookup"><span data-stu-id="a96c6-157">Open hello [Azure Portal](https://portal.azure.com) and sign in as a **Global Administrator** or **Co-admin**</span></span>

2.  <span data-ttu-id="a96c6-158">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="a96c6-158">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="a96c6-159">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="a96c6-159">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="a96c6-160">Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="a96c6-160">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="a96c6-161">Kattintson a hello **hozzáadása** hello jobb felső sarokban lévő hello a gomb **vállalati alkalmazások** panel</span><span class="sxs-lookup"><span data-stu-id="a96c6-161">click hello **Add** button at hello top-right corner on hello **Enterprise Applications** blade</span></span>

6.  <span data-ttu-id="a96c6-162">A hello **adjon meg egy nevet** hello a szövegmező **hello gyűjteményből Hozzáadás** szakaszban, hello alkalmazás hello nevét.</span><span class="sxs-lookup"><span data-stu-id="a96c6-162">In hello **Enter a name** textbox from hello **Add from hello gallery** section, type hello name of hello application.</span></span>

7.  <span data-ttu-id="a96c6-163">Válassza ki a használni kívánt tooconfigure egyszeri bejelentkezés hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="a96c6-163">Select hello application you want tooconfigure for single sign-on.</span></span>

8.  <span data-ttu-id="a96c6-164">Hello alkalmazás hozzáadása előtt módosíthatja annak nevét a hello **neve** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="a96c6-164">Before adding hello application, you can change its name from hello **Name** textbox.</span></span>

9.  <span data-ttu-id="a96c6-165">Kattintson a **Hozzáadás** tooadd hello alkalmazás gomb.</span><span class="sxs-lookup"><span data-stu-id="a96c6-165">Click **Add** button, tooadd hello application.</span></span>

<span data-ttu-id="a96c6-166">Egy rövid időszak után kell képes toosee hello alkalmazás konfigurációs panelen.</span><span class="sxs-lookup"><span data-stu-id="a96c6-166">After a short period, you be able toosee hello application’s configuration blade.</span></span>

### <a name="configure-single-sign-on-for-an-application-from-hello-azure-ad-gallery"></a><span data-ttu-id="a96c6-167">Egyszeri bejelentkezés egy alkalmazás hello Azure AD-galériából konfigurálása</span><span class="sxs-lookup"><span data-stu-id="a96c6-167">Configure single sign-on for an application from hello Azure AD gallery</span></span>

<span data-ttu-id="a96c6-168">tooconfigure egyszeri bejelentkezés egy alkalmazás kövesse az alábbi hello lépéseket:</span><span class="sxs-lookup"><span data-stu-id="a96c6-168">tooconfigure single sign-on for an application, follow hello steps below:</span></span>

1.  <span data-ttu-id="a96c6-169"><span id="_Hlk477187909" class="anchor"><span id="_Hlk477001983" class="anchor"></span></span>Nyitott hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**</span><span class="sxs-lookup"><span data-stu-id="a96c6-169"><span id="_Hlk477187909" class="anchor"><span id="_Hlk477001983" class="anchor"></span></span>Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="a96c6-170">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="a96c6-170">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="a96c6-171">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="a96c6-171">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="a96c6-172">Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="a96c6-172">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="a96c6-173">Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="a96c6-173">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="a96c6-174">Ha hello alkalmazás jelenik itt meg nem látja, akkor hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját** és set hello **megjelenítése** beállítás túl **Minden alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="a96c6-174">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="a96c6-175">Válassza ki a kívánt tooconfigure egyszeri bejelentkezés hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="a96c6-175">Select hello application you want tooconfigure single sign-on.</span></span>

7.  <span data-ttu-id="a96c6-176">Ha hello alkalmazás betölt, kattintson a hello **egyszeri bejelentkezés** hello alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="a96c6-176">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="a96c6-177">Válassza ki **SAML-alapú bejelentkezés** a hello **mód** legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="a96c6-177">Select **SAML-based Sign-on** from hello **Mode** dropdown.</span></span>

9.  <span data-ttu-id="a96c6-178">Adja meg a szükséges hello értékeket **tartomány és az URL-címeket.**</span><span class="sxs-lookup"><span data-stu-id="a96c6-178">Enter hello required values in **Domain and URLs.**</span></span> <span data-ttu-id="a96c6-179">Ezeket az értékeket kell beszerezni hello alkalmazás gyártójának segítségét.</span><span class="sxs-lookup"><span data-stu-id="a96c6-179">You should get these values from hello application vendor.</span></span>

   1. <span data-ttu-id="a96c6-180">tooconfigure hello alkalmazásra, amely a Szolgáltató által kezdeményezett egyszeri Bejelentkezést, hello bejelentkezési URL-címen egy szükséges érték.</span><span class="sxs-lookup"><span data-stu-id="a96c6-180">tooconfigure hello application as SP-initiated SSO, hello Sign on URL it’s a required value.</span></span> <span data-ttu-id="a96c6-181">Egyes alkalmazások hello azonosító értéke is szükséges.</span><span class="sxs-lookup"><span data-stu-id="a96c6-181">For some applications, hello Identifier is also a required value.</span></span>

   2. <span data-ttu-id="a96c6-182">tooconfigure hello alkalmazásra, amely a kiállító terjesztési hely által kezdeményezett egyszeri Bejelentkezést, hello válasz URL-cím szükséges érték.</span><span class="sxs-lookup"><span data-stu-id="a96c6-182">tooconfigure hello application as IdP-initiated SSO, hello Reply URL it’s a required value.</span></span> <span data-ttu-id="a96c6-183">Egyes alkalmazások hello azonosító értéke is szükséges.</span><span class="sxs-lookup"><span data-stu-id="a96c6-183">For some applications, hello Identifier is also a required value.</span></span>

10. <span data-ttu-id="a96c6-184">**Választható lehetőség:** kattintson **megjelenítése speciális URL-beállításainak** Ha azt szeretné, hogy toosee hello nem szükséges értékeket.</span><span class="sxs-lookup"><span data-stu-id="a96c6-184">**Optional:** click **Show advanced URL settings** if you want toosee hello non-required values.</span></span>

11. <span data-ttu-id="a96c6-185">A hello **felhasználói attribútumok**, válassza ki a felhasználók a hello egyedi azonosítója hello **felhasználói azonosító** legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="a96c6-185">In hello **User attributes**, select hello unique identifier for your users in hello **User Identifier** dropdown.</span></span>

12. <span data-ttu-id="a96c6-186">**Nem kötelező:** kattintson **megtekintése és szerkesztése a más felhasználói attribútumok** tooedit hello attribútumok küldött toobe toohello alkalmazás hello SAML-jogkivonat felhasználói bejelentkezéskor.</span><span class="sxs-lookup"><span data-stu-id="a96c6-186">**Optional:** click **View and edit all other user attributes** tooedit hello attributes toobe sent toohello application in hello SAML token when user sign in.</span></span>

   <span data-ttu-id="a96c6-187">tooadd attribútum:</span><span class="sxs-lookup"><span data-stu-id="a96c6-187">tooadd an attribute:</span></span>

   1. <span data-ttu-id="a96c6-188">Kattintson a **Hozzáadás attribútum**.</span><span class="sxs-lookup"><span data-stu-id="a96c6-188">click **Add attribute**.</span></span> <span data-ttu-id="a96c6-189">Adja meg a hello **neve** és hello válassza hello **érték** hello legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="a96c6-189">Enter hello **Name** and hello select hello **Value** from hello dropdown.</span></span>

   2. <span data-ttu-id="a96c6-190">Kattintson a **mentéséhez.**</span><span class="sxs-lookup"><span data-stu-id="a96c6-190">Click **Save.**</span></span> <span data-ttu-id="a96c6-191">Új attribútum hello hello táblázatban láthatja.</span><span class="sxs-lookup"><span data-stu-id="a96c6-191">You see hello new attribute in hello table.</span></span>

13. <span data-ttu-id="a96c6-192">Kattintson a **konfigurálása &lt;alkalmazásnév&gt;**  tooaccess dokumentáció tooconfigure egyszeri bejelentkezés hello alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="a96c6-192">click **Configure &lt;application name&gt;** tooaccess documentation on how tooconfigure single sign-on in hello application.</span></span> <span data-ttu-id="a96c6-193">Is hogy rendelkezik hello metadata URL-címek és a szükséges tanúsítványok toosetup SSO hello alkalmazással.</span><span class="sxs-lookup"><span data-stu-id="a96c6-193">Also, you has hello metadata URLs and certificate required toosetup SSO with hello application.</span></span>

14. <span data-ttu-id="a96c6-194">Kattintson a **mentése** toosave hello konfigurációs.</span><span class="sxs-lookup"><span data-stu-id="a96c6-194">Click **Save** toosave hello configuration.</span></span>

15. <span data-ttu-id="a96c6-195">Felhasználók hozzárendelése toohello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="a96c6-195">Assign users toohello application.</span></span>

### <a name="select-user-identifier-and-add-user-attributes-toobe-sent-toohello-application"></a><span data-ttu-id="a96c6-196">Válassza ki a felhasználói azonosítót és a felhasználói attribútumok küldött toobe toohello alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="a96c6-196">Select User Identifier and add user attributes toobe sent toohello application</span></span>

<span data-ttu-id="a96c6-197">tooselect hello felhasználói azonosítót, vagy adja hozzá a felhasználói attribútumok, hajtsa végre az alábbi lépéseket hello:</span><span class="sxs-lookup"><span data-stu-id="a96c6-197">tooselect hello User Identifier or add user attributes, follow hello steps below:</span></span>

1.  <span data-ttu-id="a96c6-198">Nyitott hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**</span><span class="sxs-lookup"><span data-stu-id="a96c6-198">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="a96c6-199">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="a96c6-199">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="a96c6-200">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="a96c6-200">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="a96c6-201">Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="a96c6-201">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="a96c6-202">Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="a96c6-202">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="a96c6-203">Ha azt szeretné, hogy itt tooshow hello alkalmazás nem látja, használja a hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját** és set hello **megjelenítése** beállítás túl **Minden alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="a96c6-203">If you do not see hello application you want tooshow up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="a96c6-204">Válassza ki az egyszeri bejelentkezés konfigurált hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="a96c6-204">Select hello application you have configured single sign-on.</span></span>

7.  <span data-ttu-id="a96c6-205">Ha hello alkalmazás betölt, kattintson a hello **egyszeri bejelentkezés** hello alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="a96c6-205">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="a96c6-206">A hello **felhasználói attribútumok** szakaszban jelölje be a felhasználók a hello egyedi azonosítója hello **felhasználói azonosító** legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="a96c6-206">Under hello **User attributes** section, select hello unique identifier for your users in hello **User Identifier** dropdown.</span></span> <span data-ttu-id="a96c6-207">hello kiválasztott beállítás toomatch hello várt értéket kell hello alkalmazás tooauthenticate hello felhasználó.</span><span class="sxs-lookup"><span data-stu-id="a96c6-207">hello selected option needs toomatch hello expected value in hello application tooauthenticate hello user.</span></span>

    >[!NOTE]
    ><span data-ttu-id="a96c6-208">Az Azure AD válassza hello formátumban hello NameID attribútum (felhasználói azonosító) kijelölt hello érték alapján, vagy a SAML AuthRequest hello hello alkalmazás által kért formátum hello.</span><span class="sxs-lookup"><span data-stu-id="a96c6-208">Azure AD select hello format for hello NameID attribute (User Identifier) based on hello value selected or hello format requested by hello application in hello SAML AuthRequest.</span></span> <span data-ttu-id="a96c6-209">További információt a Microsoft hello cikk [egyszeri bejelentkezés SAML protokoll](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) az hello rész NameIDPolicy.</span><span class="sxs-lookup"><span data-stu-id="a96c6-209">For more information visit hello article [Single Sign-On SAML protocol](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) under hello section NameIDPolicy.</span></span>
    >
    >

9.  <span data-ttu-id="a96c6-210">tooadd felhasználói attribútumok, kattintson **megtekintése és szerkesztése a más felhasználói attribútumok** tooedit hello attribútumok küldött toobe toohello alkalmazás hello SAML-jogkivonat felhasználói bejelentkezéskor.</span><span class="sxs-lookup"><span data-stu-id="a96c6-210">tooadd user attributes, click **View and edit all other user attributes** tooedit hello attributes toobe sent toohello application in hello SAML token when user sign in.</span></span>

   <span data-ttu-id="a96c6-211">tooadd attribútum:</span><span class="sxs-lookup"><span data-stu-id="a96c6-211">tooadd an attribute:</span></span>

   1. <span data-ttu-id="a96c6-212">Kattintson a **Hozzáadás attribútum**.</span><span class="sxs-lookup"><span data-stu-id="a96c6-212">click **Add attribute**.</span></span> <span data-ttu-id="a96c6-213">Adja meg a hello **neve** és hello válassza hello **érték** hello legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="a96c6-213">Enter hello **Name** and hello select hello **Value** from hello dropdown.</span></span>

   2. <span data-ttu-id="a96c6-214">Kattintson a **mentéséhez.**</span><span class="sxs-lookup"><span data-stu-id="a96c6-214">Click **Save.**</span></span> <span data-ttu-id="a96c6-215">Új attribútum hello hello táblázatban láthatja.</span><span class="sxs-lookup"><span data-stu-id="a96c6-215">You see hello new attribute in hello table.</span></span>

### <a name="download-hello-azure-ad-metadata-or-certificate"></a><span data-ttu-id="a96c6-216">Az Azure AD-metaadatok hello vagy a tanúsítvány letöltése</span><span class="sxs-lookup"><span data-stu-id="a96c6-216">Download hello Azure AD metadata or certificate</span></span>

<span data-ttu-id="a96c6-217">toodownload hello alkalmazás metaadatainak vagy Azure ad-, tanúsítvány kövesse az alábbi hello lépéseket:</span><span class="sxs-lookup"><span data-stu-id="a96c6-217">toodownload hello application metadata or certificate from Azure AD, follow hello steps below:</span></span>

1.  <span data-ttu-id="a96c6-218">Nyitott hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**</span><span class="sxs-lookup"><span data-stu-id="a96c6-218">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="a96c6-219">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="a96c6-219">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="a96c6-220">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="a96c6-220">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="a96c6-221">Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="a96c6-221">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="a96c6-222">Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="a96c6-222">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="a96c6-223">Ha hello alkalmazás jelenik itt meg nem látja, akkor hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját** és set hello **megjelenítése** beállítás túl **Minden alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="a96c6-223">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="a96c6-224">Válassza ki az egyszeri bejelentkezés konfigurált hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="a96c6-224">Select hello application you have configured single sign-on.</span></span>

7.  <span data-ttu-id="a96c6-225">Ha hello alkalmazás betölt, kattintson a hello **egyszeri bejelentkezés** hello alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="a96c6-225">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="a96c6-226">Nyissa meg túl**SAML-aláíró tanúsítványa** területen, majd kattintson a **letöltése** oszlop értékét.</span><span class="sxs-lookup"><span data-stu-id="a96c6-226">Go too**SAML Signing Certificate** section, then click **Download** column value.</span></span> <span data-ttu-id="a96c6-227">Attól függően, hogy milyen hello alkalmazásnak szüksége van, az egyszeri bejelentkezés konfigurálása lásd: vagy hello beállítás toodownload hello metaadatainak XML-kódja vagy hello tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="a96c6-227">Depending on what hello application requires configuring single sign-on, you see either hello option toodownload hello Metadata XML or hello Certificate.</span></span>

    <span data-ttu-id="a96c6-228">Az Azure AD egy URL-cím tooget hello metaadatok nem biztosít.</span><span class="sxs-lookup"><span data-stu-id="a96c6-228">Azure AD doesn’t provide a URL tooget hello metadata.</span></span> <span data-ttu-id="a96c6-229">hello metaadatok XML-fájlként csak olvasható.</span><span class="sxs-lookup"><span data-stu-id="a96c6-229">hello metadata can only be retrieved as a XML file.</span></span>

## <a name="how-tooconfigure-federated-single-sign-on-for-a-non-gallery-application"></a><span data-ttu-id="a96c6-230">Hogyan tooconfigure összevont egyszeri bejelentkezés egy nem galéria alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="a96c6-230">How tooconfigure federated single sign-on for a non-gallery application</span></span>

<span data-ttu-id="a96c6-231">toohave az Azure AD premium szüksége tooconfigure nem galéria-alkalmazás, és hello alkalmazás támogatja az SAML 2.0-s.</span><span class="sxs-lookup"><span data-stu-id="a96c6-231">tooconfigure a non-gallery application, you need toohave Azure AD premium and hello application supports SAML 2.0.</span></span> <span data-ttu-id="a96c6-232">Az Azure AD-verziókkal kapcsolatos további információkért látogasson el a [az Azure AD árképzési](https://azure.microsoft.com/pricing/details/active-directory/).</span><span class="sxs-lookup"><span data-stu-id="a96c6-232">For more information about Azure AD versions, visit [Azure AD pricing](https://azure.microsoft.com/pricing/details/active-directory/).</span></span>

-   [<span data-ttu-id="a96c6-233">Hello alkalmazás metaadatainak értékek konfigurálása az Azure AD (bejelentkezési URL-cím, az azonosítója, válasz URL-címe)</span><span class="sxs-lookup"><span data-stu-id="a96c6-233">Configure hello application’s metadata values in Azure AD (Sign on URL, Identifier, Reply URL)</span></span>](#configuring-single-sign-on)

-   [<span data-ttu-id="a96c6-234">Válassza ki a felhasználói azonosítót és a felhasználói attribútumok küldött toobe toohello alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="a96c6-234">Select User Identifier and add user attributes toobe sent toohello application</span></span>](#select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application)

-   [<span data-ttu-id="a96c6-235">Az Azure AD-metaadatok és a tanúsítvány lekérése</span><span class="sxs-lookup"><span data-stu-id="a96c6-235">Retrieve Azure AD metadata and certificate</span></span>](#download-the-azure-ad-metadata-or-certificate)

-   [<span data-ttu-id="a96c6-236">Konfigurálja az Azure AD metaadatok értékeket hello alkalmazásban (bejelentkezési URL-címet, a kibocsátó, a kijelentkezési URL-cím és a tanúsítvány)</span><span class="sxs-lookup"><span data-stu-id="a96c6-236">Configure Azure AD metadata values in hello application (Sign on URL, Issuer, Logout URL and certificate)</span></span>](#configuring-single-sign-on)

### <a name="configure-hello-applications-metadata-values-in-azure-ad-sign-on-url-identifier-reply-url"></a><span data-ttu-id="a96c6-237">Hello alkalmazás metaadatainak értékek konfigurálása az Azure AD (bejelentkezési URL-cím, az azonosítója, válasz URL-címe)</span><span class="sxs-lookup"><span data-stu-id="a96c6-237">Configure hello application’s metadata values in Azure AD (Sign on URL, Identifier, Reply URL)</span></span>

<span data-ttu-id="a96c6-238">tooconfigure egyszeri bejelentkezés egy alkalmazás, amely nincs az Azure AD-dokumentumtárban hello, kövesse az alábbi hello lépéseket:</span><span class="sxs-lookup"><span data-stu-id="a96c6-238">tooconfigure single sign-on for an application that is not in hello Azure AD gallery, follow hello steps below:</span></span>

1.  <span data-ttu-id="a96c6-239">Nyitott hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**</span><span class="sxs-lookup"><span data-stu-id="a96c6-239">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="a96c6-240">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="a96c6-240">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="a96c6-241">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="a96c6-241">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="a96c6-242">Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="a96c6-242">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="a96c6-243">Kattintson a hello **hozzáadása** hello jobb felső sarokban lévő hello a gomb **vállalati alkalmazások** panel</span><span class="sxs-lookup"><span data-stu-id="a96c6-243">click hello **Add** button at hello top-right corner on hello **Enterprise Applications** blade</span></span>

6.  <span data-ttu-id="a96c6-244">Kattintson a **nem-gyűjtemény alkalmazás** a hello **saját alkalmazás felvétele** szakasz</span><span class="sxs-lookup"><span data-stu-id="a96c6-244">click **Non-gallery application** in hello **Add your own app** section</span></span>

7.  <span data-ttu-id="a96c6-245">Adja meg hello hello alkalmazás hello **neve** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="a96c6-245">Enter hello name of hello application in hello **Name** textbox.</span></span>

8.  <span data-ttu-id="a96c6-246">Kattintson a **Hozzáadás** tooadd hello alkalmazás gomb.</span><span class="sxs-lookup"><span data-stu-id="a96c6-246">Click **Add** button, tooadd hello application.</span></span>

9.  <span data-ttu-id="a96c6-247">Ha hello alkalmazás betölt, kattintson a hello **egyszeri bejelentkezés** hello alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="a96c6-247">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

10. <span data-ttu-id="a96c6-248">Válassza ki **SAML-alapú bejelentkezés** a hello **mód** legördülő lista</span><span class="sxs-lookup"><span data-stu-id="a96c6-248">Select **SAML-based Sign-on** in hello **Mode** dropdown</span></span>

11. <span data-ttu-id="a96c6-249">Adja meg a szükséges hello értékeket **tartomány és az URL-címeket.**</span><span class="sxs-lookup"><span data-stu-id="a96c6-249">Enter hello required values in **Domain and URLs.**</span></span> <span data-ttu-id="a96c6-250">Ezeket az értékeket kell beszerezni hello alkalmazás gyártójának segítségét.</span><span class="sxs-lookup"><span data-stu-id="a96c6-250">You should get these values from hello application vendor.</span></span>

  1. <span data-ttu-id="a96c6-251">tooconfigure hello alkalmazásra, amely a kiállító terjesztési hely által kezdeményezett egyszeri Bejelentkezést, adja meg a hello válasz URL-CÍMEN és hello azonosítója.</span><span class="sxs-lookup"><span data-stu-id="a96c6-251">tooconfigure hello application as IdP-initiated SSO, enter hello Reply URL and hello Identifier.</span></span>

  2. <span data-ttu-id="a96c6-252">**Választható lehetőség:** tooconfigure hello alkalmazásra, amely a Szolgáltató által kezdeményezett egyszeri Bejelentkezést, hello bejelentkezési URL-címen egy szükséges érték.</span><span class="sxs-lookup"><span data-stu-id="a96c6-252">**Optional:** tooconfigure hello application as SP-initiated SSO, hello Sign on URL it’s a required value.</span></span>

12. <span data-ttu-id="a96c6-253">A hello **felhasználói attribútumok**, válassza ki a felhasználók a hello egyedi azonosítója hello **felhasználói azonosító** legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="a96c6-253">In hello **User attributes**, select hello unique identifier for your users in hello **User Identifier** dropdown.</span></span>

13. <span data-ttu-id="a96c6-254">**Nem kötelező:** kattintson **megtekintése és szerkesztése a más felhasználói attribútumok** tooedit hello attribútumok küldött toobe toohello alkalmazás hello SAML-jogkivonat felhasználói bejelentkezéskor.</span><span class="sxs-lookup"><span data-stu-id="a96c6-254">**Optional:** click **View and edit all other user attributes** tooedit hello attributes toobe sent toohello application in hello SAML token when user sign in.</span></span>

   <span data-ttu-id="a96c6-255">tooadd attribútum:</span><span class="sxs-lookup"><span data-stu-id="a96c6-255">tooadd an attribute:</span></span>

   1. <span data-ttu-id="a96c6-256">Kattintson a **Hozzáadás attribútum**.</span><span class="sxs-lookup"><span data-stu-id="a96c6-256">click **Add attribute**.</span></span> <span data-ttu-id="a96c6-257">Adja meg a hello **neve** és hello válassza hello **érték** hello legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="a96c6-257">Enter hello **Name** and hello select hello **Value** from hello dropdown.</span></span>

   2. <span data-ttu-id="a96c6-258">Kattintson a **mentéséhez.**</span><span class="sxs-lookup"><span data-stu-id="a96c6-258">Click **Save.**</span></span> <span data-ttu-id="a96c6-259">Új attribútum hello hello táblázatban láthatja.</span><span class="sxs-lookup"><span data-stu-id="a96c6-259">You see hello new attribute in hello table.</span></span>

14. <span data-ttu-id="a96c6-260">Kattintson a **konfigurálása &lt;alkalmazásnév&gt;**  tooaccess dokumentáció tooconfigure egyszeri bejelentkezés hello alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="a96c6-260">click **Configure &lt;application name&gt;** tooaccess documentation on how tooconfigure single sign-on in hello application.</span></span> <span data-ttu-id="a96c6-261">Is, akkor az Azure AD URL-címek és rendelkezik hello alkalmazásához szükséges tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="a96c6-261">Also, you has Azure AD URLs and certificate required for hello application.</span></span>

### <a name="select-user-identifier-and-add-user-attributes-toobe-sent-toohello-application"></a><span data-ttu-id="a96c6-262">Válassza ki a felhasználói azonosítót és a felhasználói attribútumok küldött toobe toohello alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="a96c6-262">Select User Identifier and add user attributes toobe sent toohello application</span></span>

<span data-ttu-id="a96c6-263">tooselect hello felhasználói azonosítót, vagy adja hozzá a felhasználói attribútumok, hajtsa végre az alábbi lépéseket hello:</span><span class="sxs-lookup"><span data-stu-id="a96c6-263">tooselect hello User Identifier or add user attributes, follow hello steps below:</span></span>

1.  <span data-ttu-id="a96c6-264">Nyitott hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**</span><span class="sxs-lookup"><span data-stu-id="a96c6-264">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="a96c6-265">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="a96c6-265">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="a96c6-266">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="a96c6-266">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="a96c6-267">Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="a96c6-267">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="a96c6-268">Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="a96c6-268">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="a96c6-269">Ha hello alkalmazás jelenik itt meg nem látja, akkor hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját** és set hello **megjelenítése** beállítás túl **Minden alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="a96c6-269">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="a96c6-270">Válassza ki az egyszeri bejelentkezés konfigurált hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="a96c6-270">Select hello application you have configured single sign-on.</span></span>

7.  <span data-ttu-id="a96c6-271">Ha hello alkalmazás betölt, kattintson a hello **egyszeri bejelentkezés** hello alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="a96c6-271">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="a96c6-272">A hello **felhasználói attribútumok** szakaszban jelölje be a felhasználók a hello egyedi azonosítója hello **felhasználói azonosító** legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="a96c6-272">Under hello **User attributes** section, select hello unique identifier for your users in hello **User Identifier** dropdown.</span></span> <span data-ttu-id="a96c6-273">hello kiválasztott beállítás toomatch hello várt értéket kell hello alkalmazás tooauthenticate hello felhasználó.</span><span class="sxs-lookup"><span data-stu-id="a96c6-273">hello selected option needs toomatch hello expected value in hello application tooauthenticate hello user.</span></span>

   >[!NOTE]
   ><span data-ttu-id="a96c6-274">Az Azure AD válassza hello formátumban hello NameID attribútum (felhasználói azonosító) kijelölt hello érték alapján, vagy a SAML AuthRequest hello hello alkalmazás által kért formátum hello.</span><span class="sxs-lookup"><span data-stu-id="a96c6-274">Azure AD select hello format for hello NameID attribute (User Identifier) based on hello value selected or hello format requested by hello application in hello SAML AuthRequest.</span></span> <span data-ttu-id="a96c6-275">További információt a Microsoft hello cikk [egyszeri bejelentkezés SAML protokoll](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) az hello rész NameIDPolicy.</span><span class="sxs-lookup"><span data-stu-id="a96c6-275">For more information visit hello article [Single Sign-On SAML protocol](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) under hello section NameIDPolicy.</span></span>
   >
   >

9.  <span data-ttu-id="a96c6-276">tooadd felhasználói attribútumok, kattintson **megtekintése és szerkesztése a más felhasználói attribútumok** tooedit hello attribútumok küldött toobe toohello alkalmazás hello SAML-jogkivonat felhasználói bejelentkezéskor.</span><span class="sxs-lookup"><span data-stu-id="a96c6-276">tooadd user attributes, click **View and edit all other user attributes** tooedit hello attributes toobe sent toohello application in hello SAML token when user sign in.</span></span>

   <span data-ttu-id="a96c6-277">tooadd attribútum:</span><span class="sxs-lookup"><span data-stu-id="a96c6-277">tooadd an attribute:</span></span>

   <span data-ttu-id="a96c6-278">1. Kattintson a **Hozzáadás attribútum**.</span><span class="sxs-lookup"><span data-stu-id="a96c6-278">1.click **Add attribute**.</span></span> <span data-ttu-id="a96c6-279">Adja meg a hello **neve** és hello válassza hello **érték** hello legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="a96c6-279">Enter hello **Name** and hello select hello **Value** from hello dropdown.</span></span>

   <span data-ttu-id="a96c6-280">2 kattintson **mentéséhez.**</span><span class="sxs-lookup"><span data-stu-id="a96c6-280">2 Click **Save.**</span></span> <span data-ttu-id="a96c6-281">Új attribútum hello hello táblázatban láthatja.</span><span class="sxs-lookup"><span data-stu-id="a96c6-281">You see hello new attribute in hello table.</span></span>

### <a name="download-hello-azure-ad-metadata-or-certificate"></a><span data-ttu-id="a96c6-282">Az Azure AD-metaadatok hello vagy a tanúsítvány letöltése</span><span class="sxs-lookup"><span data-stu-id="a96c6-282">Download hello Azure AD metadata or certificate</span></span>

<span data-ttu-id="a96c6-283">toodownload hello alkalmazás metaadatainak vagy Azure ad-, tanúsítvány kövesse az alábbi hello lépéseket:</span><span class="sxs-lookup"><span data-stu-id="a96c6-283">toodownload hello application metadata or certificate from Azure AD, follow hello steps below:</span></span>

1.  <span data-ttu-id="a96c6-284">Nyitott hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**</span><span class="sxs-lookup"><span data-stu-id="a96c6-284">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="a96c6-285">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="a96c6-285">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="a96c6-286">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="a96c6-286">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="a96c6-287">Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="a96c6-287">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="a96c6-288">Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="a96c6-288">click **All Applications** tooview a list of all your applications.</span></span>

   * <span data-ttu-id="a96c6-289">Ha hello alkalmazás jelenik itt meg nem látja, akkor hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját** és set hello **megjelenítése** beállítás túl **Minden alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="a96c6-289">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="a96c6-290">Válassza ki az egyszeri bejelentkezés konfigurált hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="a96c6-290">Select hello application you have configured single sign-on.</span></span>

7.  <span data-ttu-id="a96c6-291">Ha hello alkalmazás betölt, kattintson a hello **egyszeri bejelentkezés** hello alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="a96c6-291">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="a96c6-292">Nyissa meg túl**SAML-aláíró tanúsítványa** területen, majd kattintson a **letöltése** oszlop értékét.</span><span class="sxs-lookup"><span data-stu-id="a96c6-292">Go too**SAML Signing Certificate** section, then click **Download** column value.</span></span> <span data-ttu-id="a96c6-293">Attól függően, hogy milyen hello alkalmazásnak szüksége van, az egyszeri bejelentkezés konfigurálása lásd: vagy hello beállítás toodownload hello metaadatainak XML-kódja vagy hello tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="a96c6-293">Depending on what hello application requires configuring single sign-on, you see either hello option toodownload hello Metadata XML or hello Certificate.</span></span>

    <span data-ttu-id="a96c6-294">Az Azure AD egy URL-cím tooget hello metaadatok nem biztosít.</span><span class="sxs-lookup"><span data-stu-id="a96c6-294">Azure AD doesn’t provide a URL tooget hello metadata.</span></span> <span data-ttu-id="a96c6-295">hello metaadatok XML-fájlként csak olvasható.</span><span class="sxs-lookup"><span data-stu-id="a96c6-295">hello metadata can only be retrieved as a XML file.</span></span>

## <a name="how-tooconfigure-password-single-sign-on-for-an-azure-ad-gallery-application"></a><span data-ttu-id="a96c6-296">Hogyan tooconfigure jelszó egyszeri bejelentkezés az Azure AD gyűjtemény alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="a96c6-296">How tooconfigure password single sign-on for an Azure AD gallery application</span></span>

<span data-ttu-id="a96c6-297">egy alkalmazás hello Azure AD-galériából tooconfigure kell:</span><span class="sxs-lookup"><span data-stu-id="a96c6-297">tooconfigure an application from hello Azure AD gallery you need to:</span></span>

-   [<span data-ttu-id="a96c6-298">Alkalmazás hozzáadása hello Azure AD-galériából</span><span class="sxs-lookup"><span data-stu-id="a96c6-298">Add an application from hello Azure AD gallery</span></span>](#add-an-application)

-   [<span data-ttu-id="a96c6-299">Állítsa be hello alkalmazását jelszó egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="a96c6-299">Configure hello application for password single sign-on</span></span>](#configure-the-application)

### <a name="add-an-application-from-hello-azure-ad-gallery"></a><span data-ttu-id="a96c6-300">Alkalmazás hozzáadása hello Azure AD-galériából</span><span class="sxs-lookup"><span data-stu-id="a96c6-300">Add an application from hello Azure AD gallery</span></span>

<span data-ttu-id="a96c6-301">az alkalmazás a hello Azure AD-katalógusában, tooadd kövesse hello alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="a96c6-301">tooadd an application from hello Azure AD Gallery, follow hello steps below:</span></span>

1.  <span data-ttu-id="a96c6-302">Nyissa meg hello [Azure Portal](https://portal.azure.com) , és jelentkezzen be a egy **globális rendszergazda** vagy **társadminisztrátornak**</span><span class="sxs-lookup"><span data-stu-id="a96c6-302">Open hello [Azure Portal](https://portal.azure.com) and sign in as a **Global Administrator** or **Co-admin**</span></span>

2.  <span data-ttu-id="a96c6-303">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="a96c6-303">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="a96c6-304">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="a96c6-304">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="a96c6-305">Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="a96c6-305">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="a96c6-306">Kattintson a hello **hozzáadása** hello jobb felső sarokban lévő hello a gomb **vállalati alkalmazások** panel</span><span class="sxs-lookup"><span data-stu-id="a96c6-306">click hello **Add** button at hello top-right corner on hello **Enterprise Applications** blade</span></span>

6.  <span data-ttu-id="a96c6-307">A hello **adjon meg egy nevet** hello a szövegmező **hello gyűjteményből Hozzáadás** szakaszban, hello hello alkalmazás neve</span><span class="sxs-lookup"><span data-stu-id="a96c6-307">In hello **Enter a name** textbox from hello **Add from hello gallery** section, type hello name of hello application</span></span>

7.  <span data-ttu-id="a96c6-308">Válassza ki a kívánt tooconfigure az egyszeri bejelentkezés hello alkalmazás</span><span class="sxs-lookup"><span data-stu-id="a96c6-308">Select hello application you want tooconfigure for single sign-on</span></span>

8.  <span data-ttu-id="a96c6-309">Hello alkalmazás hozzáadása előtt módosíthatja annak nevét a hello **neve** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="a96c6-309">Before adding hello application, you can change its name from hello **Name** textbox.</span></span>

9.  <span data-ttu-id="a96c6-310">Kattintson a **Hozzáadás** tooadd hello alkalmazás gomb.</span><span class="sxs-lookup"><span data-stu-id="a96c6-310">Click **Add** button, tooadd hello application.</span></span>

<span data-ttu-id="a96c6-311">Egy rövid időszak után kell képes toosee hello alkalmazás konfigurációs panelen.</span><span class="sxs-lookup"><span data-stu-id="a96c6-311">After a short period, you be able toosee hello application’s configuration blade.</span></span>

### <a name="configure-hello-application-for-password-single-sign-on"></a><span data-ttu-id="a96c6-312">Állítsa be hello alkalmazását jelszó egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="a96c6-312">Configure hello application for password single sign-on</span></span>

<span data-ttu-id="a96c6-313">tooconfigure egyszeri bejelentkezés egy alkalmazás kövesse az alábbi hello lépéseket:</span><span class="sxs-lookup"><span data-stu-id="a96c6-313">tooconfigure single sign-on for an application, follow hello steps below:</span></span>

1.  <span data-ttu-id="a96c6-314">Nyitott hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**</span><span class="sxs-lookup"><span data-stu-id="a96c6-314">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="a96c6-315">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="a96c6-315">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="a96c6-316">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="a96c6-316">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="a96c6-317">Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="a96c6-317">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="a96c6-318">Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="a96c6-318">click **All Applications** tooview a list of all your applications.</span></span>

 * <span data-ttu-id="a96c6-319">Ha hello alkalmazás jelenik itt meg nem látja, akkor hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját** és set hello **megjelenítése** beállítás túl **Minden alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="a96c6-319">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="a96c6-320">Válassza ki a kívánt tooconfigure egyszeri bejelentkezés hello alkalmazás</span><span class="sxs-lookup"><span data-stu-id="a96c6-320">Select hello application you want tooconfigure single sign-on</span></span>

7.  <span data-ttu-id="a96c6-321">Ha hello alkalmazás betölt, kattintson a hello **egyszeri bejelentkezés** hello alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="a96c6-321">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="a96c6-322">Jelölje be hello mód **jelszóalapú bejelentkezés.**</span><span class="sxs-lookup"><span data-stu-id="a96c6-322">Select hello mode **Password-based Sign-on.**</span></span>

9.  <span data-ttu-id="a96c6-323">Felhasználók hozzárendelése toohello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="a96c6-323">Assign users toohello application.</span></span>

10. <span data-ttu-id="a96c6-324">Emellett is megadhatja hello felhasználó nevében hitelesítő adatok hello felhasználók hello sorát kiválasztásával, és kattintson a **frissítéséhez szükséges hitelesítő adatokat** és hello felhasználónév és jelszó megadásával hello felhasználók nevében.</span><span class="sxs-lookup"><span data-stu-id="a96c6-324">Additionally, you can also provide credentials on behalf of hello user by selecting hello rows of hello users and clicking on **Update Credentials** and entering hello username and password on behalf of hello users.</span></span> <span data-ttu-id="a96c6-325">Ellenkező esetben a felhasználók fognak felszólító tooenter hello hitelesítő adatok magukat az indítás után.</span><span class="sxs-lookup"><span data-stu-id="a96c6-325">Otherwise, users be prompted tooenter hello credentials themselves upon launch.</span></span>

## <a name="how-tooconfigure-password-single-sign-on-for-a-non-gallery-application"></a><span data-ttu-id="a96c6-326">Hogyan tooconfigure jelszó egyszeri bejelentkezés egy nem galéria alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="a96c6-326">How tooconfigure password single sign-on for a non-gallery application</span></span>

<span data-ttu-id="a96c6-327">egy alkalmazás hello Azure AD-galériából tooconfigure kell:</span><span class="sxs-lookup"><span data-stu-id="a96c6-327">tooconfigure an application from hello Azure AD gallery you need to:</span></span>

-   [<span data-ttu-id="a96c6-328">Nem galéria alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="a96c6-328">Add a non-gallery application</span></span>](#add-a-non-gallery-application)

-   [<span data-ttu-id="a96c6-329">Állítsa be hello alkalmazását jelszó egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="a96c6-329">Configure hello application for password single sign-on</span></span>](#configure-the-application-for-password-single-sign-on)

### <a name="add-a-non-gallery-application"></a><span data-ttu-id="a96c6-330">Nem galéria alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="a96c6-330">Add a non-gallery application</span></span>

<span data-ttu-id="a96c6-331">az alkalmazás a hello Azure AD-katalógusában, tooadd kövesse hello alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="a96c6-331">tooadd an application from hello Azure AD Gallery, follow hello steps below:</span></span>

1.  <span data-ttu-id="a96c6-332">Nyissa meg hello [Azure Portal](https://portal.azure.com) , és jelentkezzen be a egy **globális rendszergazda** vagy **társadminisztrátornak**</span><span class="sxs-lookup"><span data-stu-id="a96c6-332">Open hello [Azure Portal](https://portal.azure.com) and sign in as a **Global Administrator** or **Co-admin**</span></span>

2.  <span data-ttu-id="a96c6-333">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="a96c6-333">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="a96c6-334">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="a96c6-334">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="a96c6-335">Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="a96c6-335">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="a96c6-336">Kattintson a hello **hozzáadása** hello jobb felső sarokban lévő hello a gomb **vállalati alkalmazások** panel</span><span class="sxs-lookup"><span data-stu-id="a96c6-336">click hello **Add** button at hello top-right corner on hello **Enterprise Applications** blade</span></span>

6.  <span data-ttu-id="a96c6-337">Kattintson a **nem-gyűjtemény alkalmazás.**</span><span class="sxs-lookup"><span data-stu-id="a96c6-337">click **Non-gallery application.**</span></span>

7.  <span data-ttu-id="a96c6-338">Adja meg az alkalmazás nevére hello hello **neve** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="a96c6-338">Enter hello name of your application in hello **Name** textbox.</span></span> <span data-ttu-id="a96c6-339">Válassza ki **hozzáadása.**</span><span class="sxs-lookup"><span data-stu-id="a96c6-339">Select **Add.**</span></span>

<span data-ttu-id="a96c6-340">Egy rövid időszak után kell képes toosee hello alkalmazás konfigurációs panelen.</span><span class="sxs-lookup"><span data-stu-id="a96c6-340">After a short period, you be able toosee hello application’s configuration blade.</span></span>

### <a name="configure-hello-application-for-password-single-sign-on"></a><span data-ttu-id="a96c6-341">Állítsa be hello alkalmazását jelszó egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="a96c6-341">Configure hello application for password single sign-on</span></span>

<span data-ttu-id="a96c6-342">tooconfigure egyszeri bejelentkezés egy alkalmazás kövesse az alábbi hello lépéseket:</span><span class="sxs-lookup"><span data-stu-id="a96c6-342">tooconfigure single sign-on for an application, follow hello steps below:</span></span>

1.  <span data-ttu-id="a96c6-343">Nyitott hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**</span><span class="sxs-lookup"><span data-stu-id="a96c6-343">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="a96c6-344">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="a96c6-344">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="a96c6-345">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="a96c6-345">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="a96c6-346">Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="a96c6-346">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="a96c6-347">Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="a96c6-347">click **All Applications** tooview a list of all your applications.</span></span>

 * <span data-ttu-id="a96c6-348">Ha hello alkalmazás jelenik itt meg nem látja, akkor hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját** és set hello **megjelenítése** beállítás túl **Minden alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="a96c6-348">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="a96c6-349">Válassza ki a kívánt tooconfigure egyszeri bejelentkezés hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="a96c6-349">Select hello application you want tooconfigure single sign-on.</span></span>

7.  <span data-ttu-id="a96c6-350">Ha hello alkalmazás betölt, kattintson a hello **egyszeri bejelentkezés** hello alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="a96c6-350">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="a96c6-351">Jelölje be hello mód **jelszóalapú bejelentkezés.**</span><span class="sxs-lookup"><span data-stu-id="a96c6-351">Select hello mode **Password-based Sign-on.**</span></span>

9.  <span data-ttu-id="a96c6-352">Adja meg a hello **bejelentkezési URL-cím**.</span><span class="sxs-lookup"><span data-stu-id="a96c6-352">Enter hello **Sign-on URL**.</span></span> <span data-ttu-id="a96c6-353">Ez a hello URL-cím, ahol felhasználók adja meg a felhasználónevet és jelszót toosign a számára.</span><span class="sxs-lookup"><span data-stu-id="a96c6-353">This is hello URL where users enter their username and password toosign in to.</span></span> <span data-ttu-id="a96c6-354">Győződjön meg arról, mezők hello bejelentkezés láthatók hello URL-címen.</span><span class="sxs-lookup"><span data-stu-id="a96c6-354">Ensure hello sign in fields are visible at hello URL.</span></span>

10. <span data-ttu-id="a96c6-355">Felhasználók hozzárendelése toohello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="a96c6-355">Assign users toohello application.</span></span>

11. <span data-ttu-id="a96c6-356">Emellett is megadhatja hello felhasználó nevében hitelesítő adatok hello felhasználók hello sorát kiválasztásával, és kattintson a **frissítéséhez szükséges hitelesítő adatokat** és hello felhasználónév és jelszó megadásával hello felhasználók nevében.</span><span class="sxs-lookup"><span data-stu-id="a96c6-356">Additionally, you can also provide credentials on behalf of hello user by selecting hello rows of hello users and clicking on **Update Credentials** and entering hello username and password on behalf of hello users.</span></span> <span data-ttu-id="a96c6-357">Ellenkező esetben a felhasználók fognak felszólító tooenter hello hitelesítő adatok magukat az indítás után.</span><span class="sxs-lookup"><span data-stu-id="a96c6-357">Otherwise, users be prompted tooenter hello credentials themselves upon launch.</span></span>

## <a name="how-tooassign-a-user-tooan-application-directly"></a><span data-ttu-id="a96c6-358">Hogyan tooassign közvetlenül egy felhasználói tooan alkalmazást</span><span class="sxs-lookup"><span data-stu-id="a96c6-358">How tooassign a user tooan application directly</span></span>

<span data-ttu-id="a96c6-359">tooassign felhasználók tooan egy vagy több alkalmazás közvetlenül, kövesse az alábbi hello lépéseket:</span><span class="sxs-lookup"><span data-stu-id="a96c6-359">tooassign one or more users tooan application directly, follow hello steps below:</span></span>

1.  <span data-ttu-id="a96c6-360">Nyissa meg hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be a egy **globális rendszergazdája.**</span><span class="sxs-lookup"><span data-stu-id="a96c6-360">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="a96c6-361">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="a96c6-361">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="a96c6-362">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="a96c6-362">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="a96c6-363">Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="a96c6-363">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="a96c6-364">Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="a96c6-364">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="a96c6-365">Ha hello alkalmazás jelenik itt meg nem látja, akkor hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját** és set hello **megjelenítése** beállítás túl **Minden alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="a96c6-365">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="a96c6-366">Válassza ki a kívánt felhasználó toofrom hello listáját tooassign hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="a96c6-366">Select hello application you want tooassign a user toofrom hello list.</span></span>

7.  <span data-ttu-id="a96c6-367">Amikor hello alkalmazás betölt, kattintson a **felhasználók és csoportok** hello alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="a96c6-367">Once hello application loads, click **Users and Groups** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="a96c6-368">Hello kattintson **Hozzáadás** gomb felett hello **felhasználók és csoportok** lista tooopen hello **hozzáadása hozzárendelés** panelen.</span><span class="sxs-lookup"><span data-stu-id="a96c6-368">Click hello **Add** button on top of hello **Users and Groups** list tooopen hello **Add Assignment** blade.</span></span>

9.  <span data-ttu-id="a96c6-369">hello kattintson **felhasználók és csoportok** hello a választó **hozzáadása hozzárendelés** panelen.</span><span class="sxs-lookup"><span data-stu-id="a96c6-369">click hello **Users and groups** selector from hello **Add Assignment** blade.</span></span>

10. <span data-ttu-id="a96c6-370">Hello típusának **teljes név** vagy **e-mail cím** érdekli hello való hozzárendelése hello felhasználó **Keresés név vagy e-mail cím alapján** keresőmezőbe.</span><span class="sxs-lookup"><span data-stu-id="a96c6-370">Type in hello **full name** or **email address** of hello user you are interested in assigning into hello **Search by name or email address** search box.</span></span>

11. <span data-ttu-id="a96c6-371">Hello rámutat **felhasználói** a hello lista tooreveal egy **jelölőnégyzet**.</span><span class="sxs-lookup"><span data-stu-id="a96c6-371">Hover over hello **user** in hello list tooreveal a **checkbox**.</span></span> <span data-ttu-id="a96c6-372">Kattintson a hello jelölőnégyzet következő toohello felhasználó profil fénykép vagy embléma tooadd a felhasználó toohello **kijelölt** listája.</span><span class="sxs-lookup"><span data-stu-id="a96c6-372">Click hello checkbox next toohello user’s profile photo or logo tooadd your user toohello **Selected** list.</span></span>

12. <span data-ttu-id="a96c6-373">**Választható lehetőség:** Ha túl szeretné**egynél több felhasználó hozzáadása**, egy másik típus **teljes név** vagy **e-mail cím** be hello **Keresés név e-mail cím vagy** keresési mezőbe, majd kattintson a hello jelölőnégyzet tooadd a felhasználó toohello **kijelölt** listája.</span><span class="sxs-lookup"><span data-stu-id="a96c6-373">**Optional:** If you would like too**add more than one user**, type in another **full name** or **email address** into hello **Search by name or email address** search box, and click hello checkbox tooadd this user toohello **Selected** list.</span></span>

13. <span data-ttu-id="a96c6-374">Ha elkészült, válassza a felhasználók, kattintson a hello **válasszon** gomb tooadd őket felhasználók és csoportok toobe toohello listája toohello alkalmazás hozzárendelése.</span><span class="sxs-lookup"><span data-stu-id="a96c6-374">When you are finished selecting users, click hello **Select** button tooadd them toohello list of users and groups toobe assigned toohello application.</span></span>

14. <span data-ttu-id="a96c6-375">**Nem kötelező:** hello kattintson **Szerepkörválasztás** hello a választó **hozzáadása hozzárendelés** panel tooselect szerepkör tooassign toohello felhasználók kijelölt.</span><span class="sxs-lookup"><span data-stu-id="a96c6-375">**Optional:** click hello **Select Role** selector in hello **Add Assignment** blade tooselect a role tooassign toohello users you have selected.</span></span>

15. <span data-ttu-id="a96c6-376">Kattintson a hello **hozzárendelése** gomb tooassign hello alkalmazás toohello kijelölt felhasználók.</span><span class="sxs-lookup"><span data-stu-id="a96c6-376">Click hello **Assign** button tooassign hello application toohello selected users.</span></span>

<span data-ttu-id="a96c6-377">Miután rövid időn belül, kijelölt hello felhasználók kell tudni toolaunch ezeket az alkalmazásokat hello a hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="a96c6-377">After a short period, hello users you have selected be able toolaunch these applications in hello Access Panel.</span></span>

## <a name="if-these-troubleshooting-steps-do-not-hello-resolve-hello-issue"></a><span data-ttu-id="a96c6-378">Ha a lépések nem hello hello probléma megoldásához.</span><span class="sxs-lookup"><span data-stu-id="a96c6-378">If these troubleshooting steps do not hello resolve hello issue</span></span>

<span data-ttu-id="a96c6-379">Nyisson meg egy támogatási jegy hello ha rendelkezésre áll a következő információkat:</span><span class="sxs-lookup"><span data-stu-id="a96c6-379">open a support ticket with hello following information if available:</span></span>

-   <span data-ttu-id="a96c6-380">Megfelelési hiba azonosítója</span><span class="sxs-lookup"><span data-stu-id="a96c6-380">Correlation error ID</span></span>

-   <span data-ttu-id="a96c6-381">Egyszerű felhasználónév (felhasználó e-mail címe)</span><span class="sxs-lookup"><span data-stu-id="a96c6-381">UPN (user email address)</span></span>

-   <span data-ttu-id="a96c6-382">A TenantID</span><span class="sxs-lookup"><span data-stu-id="a96c6-382">TenantID</span></span>

-   <span data-ttu-id="a96c6-383">Böngésző típusa</span><span class="sxs-lookup"><span data-stu-id="a96c6-383">Browser type</span></span>

-   <span data-ttu-id="a96c6-384">Időzóna és idő/időkeretre során hiba történik.</span><span class="sxs-lookup"><span data-stu-id="a96c6-384">Time zone and time/timeframe during error occurs</span></span>

-   <span data-ttu-id="a96c6-385">Fiddler nyomkövetések</span><span class="sxs-lookup"><span data-stu-id="a96c6-385">Fiddler traces</span></span>

## <a name="next-steps"></a><span data-ttu-id="a96c6-386">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a96c6-386">Next steps</span></span>
[<span data-ttu-id="a96c6-387">Adja meg az egyszeri bejelentkezés tooyour alkalmazások alkalmazásproxyval</span><span class="sxs-lookup"><span data-stu-id="a96c6-387">Provide single sign-on tooyour apps with Application Proxy</span></span>](active-directory-application-proxy-sso-using-kcd.md)

