---
title: "bejelentkezés az alkalmazást egy mélyhivatkozás tooan aaaProblems |} Microsoft Docs"
description: "Hogyan alkalmazáshoz való hozzáférés az Azure AD mélyhivatkozás URL-címről tootroubleshoot problémák"
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
ms.openlocfilehash: dc82410001ac05895cc0244c3a89ace71bcf01a9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="problems-signing-in-tooan-application-using-a-deeplink"></a><span data-ttu-id="21630-103">Bejelentkezés az alkalmazást egy mélyhivatkozás tooan problémák</span><span class="sxs-lookup"><span data-stu-id="21630-103">Problems signing in tooan application using a deeplink</span></span>

<span data-ttu-id="21630-104">hello hozzáférési Panel egy webes portál, amely lehetővé teszi a felhasználó munkahelyi vagy iskolai fiókkal az Azure Active Directory (Azure AD) tooview és kezdő felhőalapú alkalmazások, hogy hello Azure AD-rendszergazda rendelkezik hozzáféréssel őket.</span><span class="sxs-lookup"><span data-stu-id="21630-104">hello Access Panel is a web-based portal which enables a user with a work or school account in Azure Active Directory (Azure AD) tooview and start cloud-based applications that hello Azure AD administrator has granted them access to.</span></span> 

<span data-ttu-id="21630-105">Ezeket az alkalmazásokat úgy vannak konfigurálva, hello Azure AD portálon hello felhasználó nevében.</span><span class="sxs-lookup"><span data-stu-id="21630-105">These applications are configured on behalf of hello user in hello Azure AD portal.</span></span> <span data-ttu-id="21630-106">hello alkalmazás megfelelően kell konfigurálni, és a hozzárendelt toohello felhasználó vagy csoport hello felhasználó tagja a hozzáférési Panel hello toosee hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="21630-106">hello application must be configured properly and assigned toohello user or a group hello user is a member of toosee hello application in hello Access Panel.</span></span>

<span data-ttu-id="21630-107">Mélyhivatkozással vagy a felhasználói hozzáférés URL-címei a felhasználók használhatják a jelszó-SSO alkalmazások közvetlenül a böngészők URL-címről bar tooaccess hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="21630-107">Deep links or User access URLs are links your users may use tooaccess their password-SSO applications directly from their browsers URL bars.</span></span> <span data-ttu-id="21630-108">Toothis hivatkozás útvonalon, a felhasználók fognak automatikusan anélkül, hogy először a hozzáférési Panel toogo toohello hello alkalmazásba aláírva.</span><span class="sxs-lookup"><span data-stu-id="21630-108">By navigating toothis link, users be automatically signed into hello application without having toogo toohello Access Panel first.</span></span> <span data-ttu-id="21630-109">Ez a hello felhasználók használó tooaccess az alkalmazások hello Office 365 alkalmazásindító ugyanazon kapcsolathoz.</span><span class="sxs-lookup"><span data-stu-id="21630-109">This is hello same link that users use tooaccess these applications from hello Office 365 application launcher.</span></span>

## <a name="general-issues-toocheck-first"></a><span data-ttu-id="21630-110">Általános problémák első toocheck</span><span class="sxs-lookup"><span data-stu-id="21630-110">General issues toocheck first</span></span>

-   <span data-ttu-id="21630-111">Győződjön meg arról, hogy a használatával egy **böngésző** , amely megfelel a hozzáférési Panel hello hello minimális követelményeinek.</span><span class="sxs-lookup"><span data-stu-id="21630-111">Make sure your using a **browser** that meets hello minimum requirements for hello Access Panel.</span></span>

-   <span data-ttu-id="21630-112">Ellenőrizze, hogy hello böngésző hello alkalmazás tooits hello URL-CÍMÉT hozzáadta **megbízható helyek**.</span><span class="sxs-lookup"><span data-stu-id="21630-112">Make sure hello user’s browser has added hello URL of hello application tooits **trusted sites**.</span></span>

-   <span data-ttu-id="21630-113">Ellenőrizze, hogy toocheck hello alkalmazás **konfigurált** megfelelően.</span><span class="sxs-lookup"><span data-stu-id="21630-113">Make sure toocheck hello application is **configured** correctly.</span></span>

-   <span data-ttu-id="21630-114">Ellenőrizze, hogy a hello felhasználói fiók **engedélyezett** a indított bejelentkezések.</span><span class="sxs-lookup"><span data-stu-id="21630-114">Make sure hello user’s account is **enabled** for sign ins.</span></span>

-   <span data-ttu-id="21630-115">Ellenőrizze, hogy a hello felhasználói fiók **nincs zárolva.**</span><span class="sxs-lookup"><span data-stu-id="21630-115">Make sure hello user’s account is **not locked out.**</span></span>

-   <span data-ttu-id="21630-116">Győződjön meg arról, hogy hello felhasználói **nem lejárt vagy elfelejtett jelszó.**</span><span class="sxs-lookup"><span data-stu-id="21630-116">Make sure hello user’s **password is not expired or forgotten.**</span></span>

-   <span data-ttu-id="21630-117">Győződjön meg arról, hogy **multi-factor Authentication** nem blokkolja a hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="21630-117">Make sure **Multi-Factor Authentication** is not blocking user access.</span></span>

-   <span data-ttu-id="21630-118">Győződjön meg arról, hogy egy **feltételes hozzáférési házirend** vagy **Identity Protection** házirend nem blokkolja a hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="21630-118">Make sure a **Conditional Access policy** or **Identity Protection** policy is not blocking user access.</span></span>

-   <span data-ttu-id="21630-119">Győződjön meg arról, hogy a felhasználó **hitelesítési kapcsolattartási adatai** működik-e toodate tooallow többtényezős hitelesítést vagy feltételes hozzáférési házirendek toobe lépnek érvénybe.</span><span class="sxs-lookup"><span data-stu-id="21630-119">Make sure that a user’s **authentication contact info** is up toodate tooallow Multi-Factor Authentication or Conditional Access policies toobe enforced.</span></span>

-   <span data-ttu-id="21630-120">Győződjön meg arról, hogy tooalso próbálkozzon a böngésző cookie-k törölje, majd próbálkozzon újra a toosign.</span><span class="sxs-lookup"><span data-stu-id="21630-120">Make sure tooalso try clearing your browser’s cookies and trying toosign in again.</span></span>

## <a name="checking-hello-deeplink"></a><span data-ttu-id="21630-121">Hello mélyhivatkozás ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="21630-121">Checking hello deeplink</span></span>

<span data-ttu-id="21630-122">toocheck hello megfelelő mélyhivatkozás, akkor kövesse hello alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="21630-122">toocheck if you have hello correct deeplink, follow hello steps below:</span></span>

1.  <span data-ttu-id="21630-123">Nyitott hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**</span><span class="sxs-lookup"><span data-stu-id="21630-123">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="21630-124">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="21630-124">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="21630-125">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="21630-125">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="21630-126">Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="21630-126">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="21630-127">Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="21630-127">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="21630-128">Ha hello alkalmazás jelenik itt meg nem látja, akkor hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját** és set hello **megjelenítése** beállítás túl **Minden alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="21630-128">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="21630-129">Nyitott hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**</span><span class="sxs-lookup"><span data-stu-id="21630-129">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

7.  <span data-ttu-id="21630-130">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="21630-130">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

8.  <span data-ttu-id="21630-131">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="21630-131">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

9.  <span data-ttu-id="21630-132">Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="21630-132">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

10. <span data-ttu-id="21630-133">Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="21630-133">click **All Applications** tooview a list of all your applications.</span></span>

   * <span data-ttu-id="21630-134">Ha hello alkalmazás jelenik itt meg nem látja, akkor hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját** és set hello **megjelenítése** beállítás túl **Minden alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="21630-134">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

11. <span data-ttu-id="21630-135">Válassza ki a kívánt hello ellenőrzés hello mélyhivatkozás a hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="21630-135">Select hello application you want hello check hello deeplink for.</span></span>

12. <span data-ttu-id="21630-136">Hello címke található **felhasználói URL-CÍMEN**.</span><span class="sxs-lookup"><span data-stu-id="21630-136">Find hello label **User Access URL**.</span></span> <span data-ttu-id="21630-137">Ön mélyhivatkozás URL-címet meg kell felelnie.</span><span class="sxs-lookup"><span data-stu-id="21630-137">You deeplink should match this URL.</span></span>

## <a name="how-tooinstall-hello-access-panel-browser-extension"></a><span data-ttu-id="21630-138">Hogyan tooinstall hello hozzáférési Panel bővítmény</span><span class="sxs-lookup"><span data-stu-id="21630-138">How tooinstall hello Access Panel Browser extension</span></span>

<span data-ttu-id="21630-139">tooinstall hello hozzáférési Panel bővítmény, kövesse az alábbi hello lépéseket:</span><span class="sxs-lookup"><span data-stu-id="21630-139">tooinstall hello Access Panel Browser extension, follow hello steps below:</span></span>

1.  <span data-ttu-id="21630-140">Nyissa meg hello [hozzáférési Panel](https://myapps.microsoft.com) valamelyik hello támogatott böngészők és való bejelentkezést egy **felhasználói** az Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="21630-140">Open hello [Access Panel](https://myapps.microsoft.com) in one of hello supported browsers and sign in as a **user** in your Azure AD.</span></span>

2.  <span data-ttu-id="21630-141">Kattintson egy **jelszó-SSO alkalmazás** a hozzáférési Panel hello.</span><span class="sxs-lookup"><span data-stu-id="21630-141">click a **password-SSO application** in hello Access Panel.</span></span>

3.  <span data-ttu-id="21630-142">Hello Rákérdezés azzal a kérdéssel tooinstall hello szoftver, válassza ki **telepítés**.</span><span class="sxs-lookup"><span data-stu-id="21630-142">In hello prompt asking tooinstall hello software, select **Install Now**.</span></span>

4.  <span data-ttu-id="21630-143">A böngésző alapján kell irányított toohello letöltési hivatkozását.</span><span class="sxs-lookup"><span data-stu-id="21630-143">Based on your browser you be directed toohello download link.</span></span> <span data-ttu-id="21630-144">**Adja hozzá** hello bővítmény tooyour böngésző.</span><span class="sxs-lookup"><span data-stu-id="21630-144">**Add** hello extension tooyour browser.</span></span>

5.  <span data-ttu-id="21630-145">Ha a böngésző kéri, válassza ki a tooeither **engedélyezése** vagy **engedélyezése** hello bővítmény.</span><span class="sxs-lookup"><span data-stu-id="21630-145">If your browser asks, select tooeither **Enable** or **Allow** hello extension.</span></span>

6.  <span data-ttu-id="21630-146">Korábban telepítve, **indítsa újra a** a böngésző-munkamenetet.</span><span class="sxs-lookup"><span data-stu-id="21630-146">Once installed, **restart** your browser session.</span></span>

7.  <span data-ttu-id="21630-147">Jelentkezzen be a hozzáférési Panel hello, és tekintse meg, ha **indítása** a jelszó-egyszeri bejelentkezés alkalmazásokhoz</span><span class="sxs-lookup"><span data-stu-id="21630-147">Sign in into hello Access Panel and see if you can **launch** your password-SSO applications</span></span>

<span data-ttu-id="21630-148">Az alábbi hello közvetlen hivatkozások Chrome és Firefox is letöltheti hello bővítmény:</span><span class="sxs-lookup"><span data-stu-id="21630-148">You may also download hello extension for Chrome and Firefox from hello direct links below:</span></span>

-   [<span data-ttu-id="21630-149">Chrome hozzáférési Panel bővítmény</span><span class="sxs-lookup"><span data-stu-id="21630-149">Chrome Access Panel Extension</span></span>](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

-   [<span data-ttu-id="21630-150">A Firefox hozzáférési Panel bővítmény</span><span class="sxs-lookup"><span data-stu-id="21630-150">Firefox Access Panel Extension</span></span>](https://addons.mozilla.org/firefox/addon/access-panel-extension/)

## <a name="how-tooconfigure-password-single-sign-on-for-an-azure-ad-gallery-application"></a><span data-ttu-id="21630-151">Hogyan tooconfigure jelszó egyszeri bejelentkezés az Azure AD gyűjtemény alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="21630-151">How tooconfigure password single sign-on for an Azure AD gallery application</span></span>

<span data-ttu-id="21630-152">egy alkalmazás hello Azure AD-galériából tooconfigure kell:</span><span class="sxs-lookup"><span data-stu-id="21630-152">tooconfigure an application from hello Azure AD gallery you need to:</span></span>

-   [<span data-ttu-id="21630-153">Alkalmazás hozzáadása hello Azure AD-galériából</span><span class="sxs-lookup"><span data-stu-id="21630-153">Add an application from hello Azure AD gallery</span></span>](#add-an-application-from-the-Azure-AD-gallery)

-   [<span data-ttu-id="21630-154">Állítsa be hello alkalmazását jelszó egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="21630-154">Configure hello application for password single sign-on</span></span>](#configure-the-application-for-password-single-sign-on)

### <a name="add-an-application-from-hello-azure-ad-gallery"></a><span data-ttu-id="21630-155">Alkalmazás hozzáadása hello Azure AD-galériából</span><span class="sxs-lookup"><span data-stu-id="21630-155">Add an application from hello Azure AD gallery</span></span>

<span data-ttu-id="21630-156">az alkalmazás a hello Azure AD-katalógusában, tooadd kövesse hello alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="21630-156">tooadd an application from hello Azure AD Gallery, follow hello steps below:</span></span>

1.  <span data-ttu-id="21630-157">Megnyitás hello [Azure Portal](https://portal.azure.com) , és jelentkezzen be egy **globális rendszergazda** vagy **társadminisztrátor**.</span><span class="sxs-lookup"><span data-stu-id="21630-157">Open hello [Azure Portal](https://portal.azure.com) and sign in as a **Global Administrator** or **Co-admin**.</span></span>

2.  <span data-ttu-id="21630-158">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="21630-158">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="21630-159">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="21630-159">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="21630-160">Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="21630-160">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="21630-161">Kattintson a hello **hozzáadása** hello jobb felső sarokban lévő hello a gomb **vállalati alkalmazások** panelen.</span><span class="sxs-lookup"><span data-stu-id="21630-161">click hello **Add** button at hello top-right corner on hello **Enterprise Applications** blade.</span></span>

6.  <span data-ttu-id="21630-162">A hello **adjon meg egy nevet** hello a szövegmező **hello gyűjteményből Hozzáadás** szakaszban, hello alkalmazás hello nevét.</span><span class="sxs-lookup"><span data-stu-id="21630-162">In hello **Enter a name** textbox from hello **Add from hello gallery** section, type hello name of hello application.</span></span>

7.  <span data-ttu-id="21630-163">Válassza ki a használni kívánt tooconfigure egyszeri bejelentkezés hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="21630-163">Select hello application you want tooconfigure for single sign-on.</span></span>

8.  <span data-ttu-id="21630-164">Hello alkalmazás hozzáadása előtt módosíthatja annak nevét a hello **neve** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="21630-164">Before adding hello application, you can change its name from hello **Name** textbox.</span></span>

9.  <span data-ttu-id="21630-165">Kattintson a **Hozzáadás** tooadd hello alkalmazás gomb.</span><span class="sxs-lookup"><span data-stu-id="21630-165">Click **Add** button, tooadd hello application.</span></span>

<span data-ttu-id="21630-166">Egy rövid időszak után kell képes toosee hello alkalmazás konfigurációs panelen.</span><span class="sxs-lookup"><span data-stu-id="21630-166">After a short period, you be able toosee hello application’s configuration blade.</span></span>

### <a name="configure-hello-application-for-password-single-sign-on"></a><span data-ttu-id="21630-167">Állítsa be hello alkalmazását jelszó egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="21630-167">Configure hello application for password single sign-on</span></span>

<span data-ttu-id="21630-168">tooconfigure egyszeri bejelentkezés egy alkalmazás kövesse az alábbi hello lépéseket:</span><span class="sxs-lookup"><span data-stu-id="21630-168">tooconfigure single sign-on for an application, follow hello steps below:</span></span>

1.  <span data-ttu-id="21630-169">Nyitott hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**</span><span class="sxs-lookup"><span data-stu-id="21630-169">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="21630-170">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="21630-170">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="21630-171">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="21630-171">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="21630-172">Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="21630-172">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="21630-173">Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="21630-173">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="21630-174">Ha hello alkalmazás jelenik itt meg nem látja, akkor hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját** és set hello **megjelenítése** beállítás túl **Minden alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="21630-174">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="21630-175">Válassza ki a kívánt tooconfigure egyszeri bejelentkezés hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="21630-175">Select hello application you want tooconfigure single sign-on.</span></span>

7.  <span data-ttu-id="21630-176">Ha hello alkalmazás betölt, kattintson a hello **egyszeri bejelentkezés** hello alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="21630-176">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="21630-177">Jelölje be hello mód **jelszóalapú bejelentkezés.**</span><span class="sxs-lookup"><span data-stu-id="21630-177">Select hello mode **Password-based Sign-on.**</span></span>

9.  <span data-ttu-id="21630-178">[Felhasználók hozzárendelése toohello alkalmazás](#how-to-assign-a-user-to-an-application-directly).</span><span class="sxs-lookup"><span data-stu-id="21630-178">[Assign users toohello application](#how-to-assign-a-user-to-an-application-directly).</span></span>

10. <span data-ttu-id="21630-179">Emellett is megadhatja hello felhasználó nevében hitelesítő adatok hello felhasználók hello sorát kiválasztásával, és kattintson a **frissítéséhez szükséges hitelesítő adatokat** és hello felhasználónév és jelszó megadásával hello felhasználók nevében.</span><span class="sxs-lookup"><span data-stu-id="21630-179">Additionally, you can also provide credentials on behalf of hello user by selecting hello rows of hello users and clicking on **Update Credentials** and entering hello username and password on behalf of hello users.</span></span> <span data-ttu-id="21630-180">Ellenkező esetben a felhasználók fognak felszólító tooenter hello hitelesítő adatok magukat az indítás után.</span><span class="sxs-lookup"><span data-stu-id="21630-180">Otherwise, users be prompted tooenter hello credentials themselves upon launch.</span></span>

## <a name="how-tooconfigure-password-single-sign-on-for-a-non-gallery-application"></a><span data-ttu-id="21630-181">Hogyan tooconfigure jelszó egyszeri bejelentkezés egy nem galéria alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="21630-181">How tooconfigure password single sign-on for a non-gallery application</span></span>

<span data-ttu-id="21630-182">egy alkalmazás hello Azure AD-galériából tooconfigure kell:</span><span class="sxs-lookup"><span data-stu-id="21630-182">tooconfigure an application from hello Azure AD gallery you need to:</span></span>

-   [<span data-ttu-id="21630-183">Nem galéria alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="21630-183">Add a non-gallery application</span></span>](#add-a-non-gallery-application)

-   [<span data-ttu-id="21630-184">Állítsa be hello alkalmazását jelszó egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="21630-184">Configure hello application for password single sign-on</span></span>](#configure-the-application-for-password-single-sign-on)

### <a name="add-a-non-gallery-application"></a><span data-ttu-id="21630-185">Nem galéria alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="21630-185">Add a non-gallery application</span></span>

<span data-ttu-id="21630-186">az alkalmazás a hello Azure AD-katalógusában, tooadd kövesse hello alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="21630-186">tooadd an application from hello Azure AD Gallery, follow hello steps below:</span></span>

1.  <span data-ttu-id="21630-187">Megnyitás hello [Azure Portal](https://portal.azure.com) , és jelentkezzen be egy **globális rendszergazda** vagy **társadminisztrátor**.</span><span class="sxs-lookup"><span data-stu-id="21630-187">Open hello [Azure Portal](https://portal.azure.com) and sign in as a **Global Administrator** or **Co-admin**.</span></span>

2.  <span data-ttu-id="21630-188">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="21630-188">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="21630-189">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="21630-189">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="21630-190">Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="21630-190">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="21630-191">Kattintson a hello **hozzáadása** hello jobb felső sarokban lévő hello a gomb **vállalati alkalmazások** panelen.</span><span class="sxs-lookup"><span data-stu-id="21630-191">click hello **Add** button at hello top-right corner on hello **Enterprise Applications** blade.</span></span>

6.  <span data-ttu-id="21630-192">Kattintson a **nem-gyűjtemény alkalmazás.**</span><span class="sxs-lookup"><span data-stu-id="21630-192">click **Non-gallery application.**</span></span>

7.  <span data-ttu-id="21630-193">Adja meg az alkalmazás nevére hello hello **neve** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="21630-193">Enter hello name of your application in hello **Name** textbox.</span></span> <span data-ttu-id="21630-194">Válassza ki **hozzáadása.**</span><span class="sxs-lookup"><span data-stu-id="21630-194">Select **Add.**</span></span>

<span data-ttu-id="21630-195">Egy rövid időszak után kell képes toosee hello alkalmazás konfigurációs panelen.</span><span class="sxs-lookup"><span data-stu-id="21630-195">After a short period, you be able toosee hello application’s configuration blade.</span></span>

### <a name="configure-hello-application-for-password-single-sign-on"></a><span data-ttu-id="21630-196">Állítsa be hello alkalmazását jelszó egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="21630-196">Configure hello application for password single sign-on</span></span>

<span data-ttu-id="21630-197">tooconfigure egyszeri bejelentkezés egy alkalmazás kövesse az alábbi hello lépéseket:</span><span class="sxs-lookup"><span data-stu-id="21630-197">tooconfigure single sign-on for an application, follow hello steps below:</span></span>

1.  <span data-ttu-id="21630-198">Nyitott hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**</span><span class="sxs-lookup"><span data-stu-id="21630-198">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="21630-199">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="21630-199">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="21630-200">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="21630-200">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="21630-201">Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="21630-201">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="21630-202">Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="21630-202">click **All Applications** tooview a list of all your applications.</span></span>

    1.  <span data-ttu-id="21630-203">Ha hello alkalmazás jelenik itt meg nem látja, akkor hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját** és set hello **megjelenítése** beállítás túl **Minden alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="21630-203">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="21630-204">Válassza ki a kívánt tooconfigure egyszeri bejelentkezés hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="21630-204">Select hello application you want tooconfigure single sign-on.</span></span>

7.  <span data-ttu-id="21630-205">Ha hello alkalmazás betölt, kattintson a hello **egyszeri bejelentkezés** hello alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="21630-205">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="21630-206">Jelölje be hello mód **jelszóalapú bejelentkezés.**</span><span class="sxs-lookup"><span data-stu-id="21630-206">Select hello mode **Password-based Sign-on.**</span></span>

9.  <span data-ttu-id="21630-207">Adja meg a hello **bejelentkezési URL-cím**.</span><span class="sxs-lookup"><span data-stu-id="21630-207">Enter hello **Sign-on URL**.</span></span> <span data-ttu-id="21630-208">Ez a hello URL-cím, ahol felhasználók adja meg a felhasználónevet és jelszót toosign a számára.</span><span class="sxs-lookup"><span data-stu-id="21630-208">This is hello URL where users enter their username and password toosign in to.</span></span> <span data-ttu-id="21630-209">Győződjön meg arról, mezők hello bejelentkezés láthatók hello URL-címen.</span><span class="sxs-lookup"><span data-stu-id="21630-209">Ensure hello sign in fields are visible at hello URL.</span></span>

10. <span data-ttu-id="21630-210">Felhasználók hozzárendelése toohello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="21630-210">Assign users toohello application.</span></span>

11. <span data-ttu-id="21630-211">Emellett is megadhatja hello felhasználó nevében hitelesítő adatok hello felhasználók hello sorát kiválasztásával, és kattintson a **frissítéséhez szükséges hitelesítő adatokat** és hello felhasználónév és jelszó megadásával hello felhasználók nevében.</span><span class="sxs-lookup"><span data-stu-id="21630-211">Additionally, you can also provide credentials on behalf of hello user by selecting hello rows of hello users and clicking on **Update Credentials** and entering hello username and password on behalf of hello users.</span></span> <span data-ttu-id="21630-212">Ellenkező esetben a felhasználók fognak felszólító tooenter hello hitelesítő adatok magukat az indítás után.</span><span class="sxs-lookup"><span data-stu-id="21630-212">Otherwise, users be prompted tooenter hello credentials themselves upon launch.</span></span>

## <a name="how-tooassign-a-user-tooan-application-directly"></a><span data-ttu-id="21630-213">Hogyan tooassign közvetlenül egy felhasználói tooan alkalmazást</span><span class="sxs-lookup"><span data-stu-id="21630-213">How tooassign a user tooan application directly</span></span>

<span data-ttu-id="21630-214">tooassign felhasználók tooan egy vagy több alkalmazás közvetlenül, kövesse az alábbi hello lépéseket:</span><span class="sxs-lookup"><span data-stu-id="21630-214">tooassign one or more users tooan application directly, follow hello steps below:</span></span>

1.  <span data-ttu-id="21630-215">Nyissa meg hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be a egy **globális rendszergazdája.**</span><span class="sxs-lookup"><span data-stu-id="21630-215">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="21630-216">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="21630-216">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="21630-217">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="21630-217">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="21630-218">Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="21630-218">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="21630-219">Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="21630-219">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="21630-220">Ha hello alkalmazás jelenik itt meg nem látja, akkor hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját** és set hello **megjelenítése** beállítás túl **Minden alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="21630-220">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="21630-221">Válassza ki a kívánt felhasználó toofrom hello listáját tooassign hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="21630-221">Select hello application you want tooassign a user toofrom hello list.</span></span>

7.  <span data-ttu-id="21630-222">Amikor hello alkalmazás betölt, kattintson a **felhasználók és csoportok** hello alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="21630-222">Once hello application loads, click **Users and Groups** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="21630-223">Hello kattintson **Hozzáadás** gomb felett hello **felhasználók és csoportok** lista tooopen hello **hozzáadása hozzárendelés** panelen.</span><span class="sxs-lookup"><span data-stu-id="21630-223">Click hello **Add** button on top of hello **Users and Groups** list tooopen hello **Add Assignment** blade.</span></span>

9.  <span data-ttu-id="21630-224">hello kattintson **felhasználók és csoportok** hello a választó **hozzáadása hozzárendelés** panelen.</span><span class="sxs-lookup"><span data-stu-id="21630-224">click hello **Users and groups** selector from hello **Add Assignment** blade.</span></span>

10. <span data-ttu-id="21630-225">Hello típusának **teljes név** vagy **e-mail cím** érdekli hello való hozzárendelése hello felhasználó **Keresés név vagy e-mail cím alapján** keresőmezőbe.</span><span class="sxs-lookup"><span data-stu-id="21630-225">Type in hello **full name** or **email address** of hello user you are interested in assigning into hello **Search by name or email address** search box.</span></span>

11. <span data-ttu-id="21630-226">Hello rámutat **felhasználói** a hello lista tooreveal egy **jelölőnégyzet**.</span><span class="sxs-lookup"><span data-stu-id="21630-226">Hover over hello **user** in hello list tooreveal a **checkbox**.</span></span> <span data-ttu-id="21630-227">Kattintson a hello jelölőnégyzet következő toohello felhasználó profil fénykép vagy embléma tooadd a felhasználó toohello **kijelölt** listája.</span><span class="sxs-lookup"><span data-stu-id="21630-227">Click hello checkbox next toohello user’s profile photo or logo tooadd your user toohello **Selected** list.</span></span>

12. <span data-ttu-id="21630-228">**Választható lehetőség:** Ha túl szeretné**egynél több felhasználó hozzáadása**, egy másik típus **teljes név** vagy **e-mail cím** be hello **Keresés név e-mail cím vagy** keresési mezőbe, majd kattintson a hello jelölőnégyzet tooadd a felhasználó toohello **kijelölt** listája.</span><span class="sxs-lookup"><span data-stu-id="21630-228">**Optional:** If you would like too**add more than one user**, type in another **full name** or **email address** into hello **Search by name or email address** search box, and click hello checkbox tooadd this user toohello **Selected** list.</span></span>

13. <span data-ttu-id="21630-229">Ha elkészült, válassza a felhasználók, kattintson a hello **válasszon** gomb tooadd őket felhasználók és csoportok toobe toohello listája toohello alkalmazás hozzárendelése.</span><span class="sxs-lookup"><span data-stu-id="21630-229">When you are finished selecting users, click hello **Select** button tooadd them toohello list of users and groups toobe assigned toohello application.</span></span>

14. <span data-ttu-id="21630-230">**Nem kötelező:** hello kattintson **Szerepkörválasztás** hello a választó **hozzáadása hozzárendelés** panel tooselect szerepkör tooassign toohello felhasználók kijelölt.</span><span class="sxs-lookup"><span data-stu-id="21630-230">**Optional:** click hello **Select Role** selector in hello **Add Assignment** blade tooselect a role tooassign toohello users you have selected.</span></span>

15. <span data-ttu-id="21630-231">Kattintson a hello **hozzárendelése** gomb tooassign hello alkalmazás toohello kijelölt felhasználók.</span><span class="sxs-lookup"><span data-stu-id="21630-231">Click hello **Assign** button tooassign hello application toohello selected users.</span></span>

<span data-ttu-id="21630-232">Miután rövid időn belül, kijelölt hello felhasználók kell tudni toolaunch ezeket az alkalmazásokat hello a hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="21630-232">After a short period, hello users you have selected be able toolaunch these applications in hello Access Panel.</span></span>

## <a name="if-these-troubleshooting-steps-do-not-hello-resolve-hello-issue"></a><span data-ttu-id="21630-233">Ha a lépések nem hello oldja meg a hello problémát.</span><span class="sxs-lookup"><span data-stu-id="21630-233">If these troubleshooting steps do not hello resolve hello issue.</span></span> 

<span data-ttu-id="21630-234">Nyisson meg egy támogatási jegy hello ha rendelkezésre áll a következő információkat:</span><span class="sxs-lookup"><span data-stu-id="21630-234">open a support ticket with hello following information if available:</span></span>

-   <span data-ttu-id="21630-235">Megfelelési hiba azonosítója</span><span class="sxs-lookup"><span data-stu-id="21630-235">Correlation error ID</span></span>

-   <span data-ttu-id="21630-236">Egyszerű felhasználónév (felhasználó e-mail címe)</span><span class="sxs-lookup"><span data-stu-id="21630-236">UPN (user email address)</span></span>

-   <span data-ttu-id="21630-237">A TenantID</span><span class="sxs-lookup"><span data-stu-id="21630-237">TenantID</span></span>

-   <span data-ttu-id="21630-238">Böngésző típusa</span><span class="sxs-lookup"><span data-stu-id="21630-238">Browser type</span></span>

-   <span data-ttu-id="21630-239">Időzóna és idő/időkeretre során hiba történik.</span><span class="sxs-lookup"><span data-stu-id="21630-239">Time zone and time/timeframe during error occurs</span></span>

-   <span data-ttu-id="21630-240">Fiddler nyomkövetések</span><span class="sxs-lookup"><span data-stu-id="21630-240">Fiddler traces</span></span>

## <a name="next-steps"></a><span data-ttu-id="21630-241">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="21630-241">Next steps</span></span>
[<span data-ttu-id="21630-242">Adja meg az egyszeri bejelentkezés tooyour alkalmazások alkalmazásproxyval</span><span class="sxs-lookup"><span data-stu-id="21630-242">Provide single sign-on tooyour apps with Application Proxy</span></span>](active-directory-application-proxy-sso-using-kcd.md)
