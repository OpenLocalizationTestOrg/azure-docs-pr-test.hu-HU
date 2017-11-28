---
title: "Bejelentkezés egy mélyhivatkozás használó alkalmazások problémák |} Microsoft Docs"
description: "Alkalmazáshoz való hozzáférés az Azure AD mélyhivatkozás URL-címről kapcsolatos problémák elhárítása"
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
ms.openlocfilehash: 798015ab68afc65378cffc75afec9c7f91fc1926
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="problems-signing-in-to-an-application-using-a-deeplink"></a><span data-ttu-id="755a2-103">A mélyhivatkozási használó alkalmazások történő bejelentkezés problémák</span><span class="sxs-lookup"><span data-stu-id="755a2-103">Problems signing in to an application using a deeplink</span></span>

<span data-ttu-id="755a2-104">A hozzáférési Panel egy webes portál, amely lehetővé teszi a felhasználó munkahelyi vagy iskolai fiókkal az Azure Active Directory (Azure AD) a megtekintése, és indítsa el a felhőalapú alkalmazások, hogy az Azure AD-rendszergazda engedélyezte őket hozzáférés annak.</span><span class="sxs-lookup"><span data-stu-id="755a2-104">The Access Panel is a web-based portal which enables a user with a work or school account in Azure Active Directory (Azure AD) to view and start cloud-based applications that the Azure AD administrator has granted them access to.</span></span> 

<span data-ttu-id="755a2-105">Ezeket az alkalmazásokat úgy vannak konfigurálva, az Azure AD portálon a felhasználó nevében.</span><span class="sxs-lookup"><span data-stu-id="755a2-105">These applications are configured on behalf of the user in the Azure AD portal.</span></span> <span data-ttu-id="755a2-106">Az alkalmazás megfelelően konfigurálva legyen, és a felhasználó vagy egy csoport, a felhasználó tagja a hozzáférési panelen az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="755a2-106">The application must be configured properly and assigned to the user or a group the user is a member of to see the application in the Access Panel.</span></span>

<span data-ttu-id="755a2-107">Mélyhivatkozással vagy a felhasználói hozzáférés URL-címei a felhasználók használhatják a jelszó-SSO alkalmazások közvetlenül elérje a böngészők URL-cím sávok hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="755a2-107">Deep links or User access URLs are links your users may use to access their password-SSO applications directly from their browsers URL bars.</span></span> <span data-ttu-id="755a2-108">Nyissa meg ezt a hivatkozást, a felhasználók fognak automatikusan nélkül a hozzáférési panel először az alkalmazás aláírt.</span><span class="sxs-lookup"><span data-stu-id="755a2-108">By navigating to this link, users be automatically signed into the application without having to go to the Access Panel first.</span></span> <span data-ttu-id="755a2-109">Ez az egyazon kapcsolat, amely felhasználói az Office 365 alkalmazásindító ezek az alkalmazások eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="755a2-109">This is the same link that users use to access these applications from the Office 365 application launcher.</span></span>

## <a name="general-issues-to-check-first"></a><span data-ttu-id="755a2-110">Először ellenőrizze a általános problémák</span><span class="sxs-lookup"><span data-stu-id="755a2-110">General issues to check first</span></span>

-   <span data-ttu-id="755a2-111">Győződjön meg arról, hogy a használatával egy **böngésző** , amely megfelel a hozzáférési Panel minimális követelményeinek.</span><span class="sxs-lookup"><span data-stu-id="755a2-111">Make sure your using a **browser** that meets the minimum requirements for the Access Panel.</span></span>

-   <span data-ttu-id="755a2-112">Győződjön meg arról, hogy a felhasználó böngészőjéből az alkalmazás URL-CÍMÉT hozzáadta a **megbízható helyek**.</span><span class="sxs-lookup"><span data-stu-id="755a2-112">Make sure the user’s browser has added the URL of the application to its **trusted sites**.</span></span>

-   <span data-ttu-id="755a2-113">Ellenőrizze, hogy az alkalmazás **konfigurált** megfelelően.</span><span class="sxs-lookup"><span data-stu-id="755a2-113">Make sure to check the application is **configured** correctly.</span></span>

-   <span data-ttu-id="755a2-114">Győződjön meg arról, hogy a felhasználói fiók **engedélyezett** a indított bejelentkezések.</span><span class="sxs-lookup"><span data-stu-id="755a2-114">Make sure the user’s account is **enabled** for sign ins.</span></span>

-   <span data-ttu-id="755a2-115">Győződjön meg arról, hogy a felhasználói fiók **nincs zárolva.**</span><span class="sxs-lookup"><span data-stu-id="755a2-115">Make sure the user’s account is **not locked out.**</span></span>

-   <span data-ttu-id="755a2-116">Győződjön meg arról, hogy a felhasználó **nem lejárt vagy elfelejtett jelszó.**</span><span class="sxs-lookup"><span data-stu-id="755a2-116">Make sure the user’s **password is not expired or forgotten.**</span></span>

-   <span data-ttu-id="755a2-117">Győződjön meg arról, hogy **multi-factor Authentication** nem blokkolja a hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="755a2-117">Make sure **Multi-Factor Authentication** is not blocking user access.</span></span>

-   <span data-ttu-id="755a2-118">Győződjön meg arról, hogy egy **feltételes hozzáférési házirend** vagy **Identity Protection** házirend nem blokkolja a hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="755a2-118">Make sure a **Conditional Access policy** or **Identity Protection** policy is not blocking user access.</span></span>

-   <span data-ttu-id="755a2-119">Győződjön meg arról, hogy a felhasználó **hitelesítési kapcsolattartási adatai** esetén a multi-factor Authentication vagy feltételes hozzáférési házirendek kényszerítését dátum.</span><span class="sxs-lookup"><span data-stu-id="755a2-119">Make sure that a user’s **authentication contact info** is up to date to allow Multi-Factor Authentication or Conditional Access policies to be enforced.</span></span>

-   <span data-ttu-id="755a2-120">A győződjön meg arról is megpróbálja törlésével a böngésző cookie-kat, majd próbálkozzon újból bejelentkezni.</span><span class="sxs-lookup"><span data-stu-id="755a2-120">Make sure to also try clearing your browser’s cookies and trying to sign in again.</span></span>

## <a name="checking-the-deeplink"></a><span data-ttu-id="755a2-121">A mélyhivatkozási ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="755a2-121">Checking the deeplink</span></span>

<span data-ttu-id="755a2-122">Ellenőrizze, hogy van-e a megfelelő mélyhivatkozás, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="755a2-122">To check if you have the correct deeplink, follow the steps below:</span></span>

1.  <span data-ttu-id="755a2-123">Nyissa meg a [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**</span><span class="sxs-lookup"><span data-stu-id="755a2-123">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="755a2-124">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="755a2-124">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="755a2-125">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="755a2-125">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="755a2-126">Kattintson a **vállalati alkalmazások** az Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="755a2-126">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="755a2-127">Kattintson a **összes alkalmazás** az alkalmazások listájának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="755a2-127">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="755a2-128">Ha azt szeretné, hogy itt megjelennek az alkalmazás nem látja, használja a **szűrő** vezérlő tetején a **összes alkalmazások listáját** és állítsa be a **megjelenítése** lehetőséggel **összes alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="755a2-128">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="755a2-129">Nyissa meg a [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**</span><span class="sxs-lookup"><span data-stu-id="755a2-129">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

7.  <span data-ttu-id="755a2-130">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="755a2-130">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

8.  <span data-ttu-id="755a2-131">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="755a2-131">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

9.  <span data-ttu-id="755a2-132">Kattintson a **vállalati alkalmazások** az Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="755a2-132">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

10. <span data-ttu-id="755a2-133">Kattintson a **összes alkalmazás** az alkalmazások listájának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="755a2-133">click **All Applications** to view a list of all your applications.</span></span>

   * <span data-ttu-id="755a2-134">Ha azt szeretné, hogy itt megjelennek az alkalmazás nem látja, használja a **szűrő** vezérlő tetején a **összes alkalmazások listáját** és állítsa be a **megjelenítése** lehetőséggel **összes alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="755a2-134">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

11. <span data-ttu-id="755a2-135">A mélyhivatkozási az ellenőrzés kívánt alkalmazás kiválasztása.</span><span class="sxs-lookup"><span data-stu-id="755a2-135">Select the application you want the check the deeplink for.</span></span>

12. <span data-ttu-id="755a2-136">A címke található **felhasználói URL-CÍMEN**.</span><span class="sxs-lookup"><span data-stu-id="755a2-136">Find the label **User Access URL**.</span></span> <span data-ttu-id="755a2-137">Ön mélyhivatkozás URL-címet meg kell felelnie.</span><span class="sxs-lookup"><span data-stu-id="755a2-137">You deeplink should match this URL.</span></span>

## <a name="how-to-install-the-access-panel-browser-extension"></a><span data-ttu-id="755a2-138">A hozzáférési Panel bővítmény telepítése</span><span class="sxs-lookup"><span data-stu-id="755a2-138">How to install the Access Panel Browser extension</span></span>

<span data-ttu-id="755a2-139">A hozzáférési Panel bővítmény telepítéséhez kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="755a2-139">To install the Access Panel Browser extension, follow the steps below:</span></span>

1.  <span data-ttu-id="755a2-140">Nyissa meg a [hozzáférési Panel](https://myapps.microsoft.com) valamelyik támogatott böngészők és való bejelentkezést egy **felhasználói** az Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="755a2-140">Open the [Access Panel](https://myapps.microsoft.com) in one of the supported browsers and sign in as a **user** in your Azure AD.</span></span>

2.  <span data-ttu-id="755a2-141">Kattintson egy **jelszó-SSO alkalmazás** a hozzáférési panelen.</span><span class="sxs-lookup"><span data-stu-id="755a2-141">click a **password-SSO application** in the Access Panel.</span></span>

3.  <span data-ttu-id="755a2-142">Válassza ki a szoftver telepítéséhez megkérdezi **telepítés**.</span><span class="sxs-lookup"><span data-stu-id="755a2-142">In the prompt asking to install the software, select **Install Now**.</span></span>

4.  <span data-ttu-id="755a2-143">A böngésző alapján kell irányítani a letöltési hivatkozás.</span><span class="sxs-lookup"><span data-stu-id="755a2-143">Based on your browser you be directed to the download link.</span></span> <span data-ttu-id="755a2-144">**Adja hozzá** az bővítményt, hogy a böngészőben.</span><span class="sxs-lookup"><span data-stu-id="755a2-144">**Add** the extension to your browser.</span></span>

5.  <span data-ttu-id="755a2-145">Ha a böngésző, válassza ki vagy **engedélyezése** vagy **engedélyezése** a bővítményt.</span><span class="sxs-lookup"><span data-stu-id="755a2-145">If your browser asks, select to either **Enable** or **Allow** the extension.</span></span>

6.  <span data-ttu-id="755a2-146">Korábban telepítve, **indítsa újra a** a böngésző-munkamenetet.</span><span class="sxs-lookup"><span data-stu-id="755a2-146">Once installed, **restart** your browser session.</span></span>

7.  <span data-ttu-id="755a2-147">Jelentkezzen be a hozzáférési panelre, és tekintse meg, ha **indítása** a jelszó-egyszeri bejelentkezés alkalmazásokhoz</span><span class="sxs-lookup"><span data-stu-id="755a2-147">Sign in into the Access Panel and see if you can **launch** your password-SSO applications</span></span>

<span data-ttu-id="755a2-148">Az alábbi közvetlen hivatkozások közül a Chrome és Firefox is letöltheti a bővítmény:</span><span class="sxs-lookup"><span data-stu-id="755a2-148">You may also download the extension for Chrome and Firefox from the direct links below:</span></span>

-   [<span data-ttu-id="755a2-149">Chrome hozzáférési Panel bővítmény</span><span class="sxs-lookup"><span data-stu-id="755a2-149">Chrome Access Panel Extension</span></span>](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

-   [<span data-ttu-id="755a2-150">A Firefox hozzáférési Panel bővítmény</span><span class="sxs-lookup"><span data-stu-id="755a2-150">Firefox Access Panel Extension</span></span>](https://addons.mozilla.org/firefox/addon/access-panel-extension/)

## <a name="how-to-configure-password-single-sign-on-for-an-azure-ad-gallery-application"></a><span data-ttu-id="755a2-151">Jelszó egyszeri bejelentkezés az Azure AD gyűjtemény alkalmazás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="755a2-151">How to configure password single sign-on for an Azure AD gallery application</span></span>

<span data-ttu-id="755a2-152">Egy alkalmazás kell az Azure AD-galériából konfigurálása:</span><span class="sxs-lookup"><span data-stu-id="755a2-152">To configure an application from the Azure AD gallery you need to:</span></span>

-   [<span data-ttu-id="755a2-153">Alkalmazás hozzáadása az Azure AD-galériából</span><span class="sxs-lookup"><span data-stu-id="755a2-153">Add an application from the Azure AD gallery</span></span>](#add-an-application-from-the-Azure-AD-gallery)

-   [<span data-ttu-id="755a2-154">Állítsa be az alkalmazását jelszó egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="755a2-154">Configure the application for password single sign-on</span></span>](#configure-the-application-for-password-single-sign-on)

### <a name="add-an-application-from-the-azure-ad-gallery"></a><span data-ttu-id="755a2-155">Alkalmazás hozzáadása az Azure AD-galériából</span><span class="sxs-lookup"><span data-stu-id="755a2-155">Add an application from the Azure AD gallery</span></span>

<span data-ttu-id="755a2-156">Hozzáadhat egy alkalmazást az Azure AD-gyűjteményből, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="755a2-156">To add an application from the Azure AD Gallery, follow the steps below:</span></span>

1.  <span data-ttu-id="755a2-157">Nyissa meg a [Azure Portal](https://portal.azure.com) , és jelentkezzen be egy **globális rendszergazda** vagy **társadminisztrátor**.</span><span class="sxs-lookup"><span data-stu-id="755a2-157">Open the [Azure Portal](https://portal.azure.com) and sign in as a **Global Administrator** or **Co-admin**.</span></span>

2.  <span data-ttu-id="755a2-158">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="755a2-158">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="755a2-159">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="755a2-159">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="755a2-160">Kattintson a **vállalati alkalmazások** az Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="755a2-160">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="755a2-161">Kattintson a **Hozzáadás** gombra a jobb felső sarkában a **vállalati alkalmazások** panelen.</span><span class="sxs-lookup"><span data-stu-id="755a2-161">click the **Add** button at the top-right corner on the **Enterprise Applications** blade.</span></span>

6.  <span data-ttu-id="755a2-162">Az a **adjon meg egy nevet** a szövegmező a **hozzáadása a gyűjteményből** területen írja be az alkalmazás nevét.</span><span class="sxs-lookup"><span data-stu-id="755a2-162">In the **Enter a name** textbox from the **Add from the gallery** section, type the name of the application.</span></span>

7.  <span data-ttu-id="755a2-163">Válassza ki az alkalmazást, az egyszeri bejelentkezés konfigurálni szeretné.</span><span class="sxs-lookup"><span data-stu-id="755a2-163">Select the application you want to configure for single sign-on.</span></span>

8.  <span data-ttu-id="755a2-164">Ad hozzá az alkalmazást, mielőtt a nevét módosíthatja a **neve** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="755a2-164">Before adding the application, you can change its name from the **Name** textbox.</span></span>

9.  <span data-ttu-id="755a2-165">Kattintson a **Hozzáadás** gombra, vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="755a2-165">Click **Add** button, to add the application.</span></span>

<span data-ttu-id="755a2-166">Egy rövid időszak után el az alkalmazás konfigurációs panelen láthatók.</span><span class="sxs-lookup"><span data-stu-id="755a2-166">After a short period, you be able to see the application’s configuration blade.</span></span>

### <a name="configure-the-application-for-password-single-sign-on"></a><span data-ttu-id="755a2-167">Állítsa be az alkalmazását jelszó egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="755a2-167">Configure the application for password single sign-on</span></span>

<span data-ttu-id="755a2-168">Egyszeri bejelentkezés egy alkalmazás konfigurálásához kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="755a2-168">To configure single sign-on for an application, follow the steps below:</span></span>

1.  <span data-ttu-id="755a2-169">Nyissa meg a [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**</span><span class="sxs-lookup"><span data-stu-id="755a2-169">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="755a2-170">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="755a2-170">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="755a2-171">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="755a2-171">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="755a2-172">Kattintson a **vállalati alkalmazások** az Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="755a2-172">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="755a2-173">Kattintson a **összes alkalmazás** az alkalmazások listájának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="755a2-173">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="755a2-174">Ha azt szeretné, hogy itt megjelennek az alkalmazás nem látja, használja a **szűrő** vezérlő tetején a **összes alkalmazások listáját** és állítsa be a **megjelenítése** lehetőséggel **összes alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="755a2-174">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="755a2-175">Válassza ki az egyszeri bejelentkezés konfigurálni kívánt alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="755a2-175">Select the application you want to configure single sign-on.</span></span>

7.  <span data-ttu-id="755a2-176">Ha az alkalmazás betölt, kattintson a **egyszeri bejelentkezés** az alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="755a2-176">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="755a2-177">Válassza ki a módot **jelszóalapú bejelentkezés.**</span><span class="sxs-lookup"><span data-stu-id="755a2-177">Select the mode **Password-based Sign-on.**</span></span>

9.  <span data-ttu-id="755a2-178">[Felhasználók hozzárendelése az alkalmazás](#how-to-assign-a-user-to-an-application-directly).</span><span class="sxs-lookup"><span data-stu-id="755a2-178">[Assign users to the application](#how-to-assign-a-user-to-an-application-directly).</span></span>

10. <span data-ttu-id="755a2-179">Emellett is megadhatja a felhasználó nevében hitelesítő adatok a felhasználók a sorok kijelöléséhez és parancsával **frissítéséhez szükséges hitelesítő adatokat** , majd gépelje be a felhasználónevet és jelszót a felhasználók nevében.</span><span class="sxs-lookup"><span data-stu-id="755a2-179">Additionally, you can also provide credentials on behalf of the user by selecting the rows of the users and clicking on **Update Credentials** and entering the username and password on behalf of the users.</span></span> <span data-ttu-id="755a2-180">Ellenkező esetben megkérdezi a felhasználókat a hitelesítő adatok magukat az indítás után.</span><span class="sxs-lookup"><span data-stu-id="755a2-180">Otherwise, users be prompted to enter the credentials themselves upon launch.</span></span>

## <a name="how-to-configure-password-single-sign-on-for-a-non-gallery-application"></a><span data-ttu-id="755a2-181">Jelszó egyszeri bejelentkezés nem galéria alkalmazások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="755a2-181">How to configure password single sign-on for a non-gallery application</span></span>

<span data-ttu-id="755a2-182">Egy alkalmazás kell az Azure AD-galériából konfigurálása:</span><span class="sxs-lookup"><span data-stu-id="755a2-182">To configure an application from the Azure AD gallery you need to:</span></span>

-   [<span data-ttu-id="755a2-183">Nem galéria alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="755a2-183">Add a non-gallery application</span></span>](#add-a-non-gallery-application)

-   [<span data-ttu-id="755a2-184">Állítsa be az alkalmazását jelszó egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="755a2-184">Configure the application for password single sign-on</span></span>](#configure-the-application-for-password-single-sign-on)

### <a name="add-a-non-gallery-application"></a><span data-ttu-id="755a2-185">Nem galéria alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="755a2-185">Add a non-gallery application</span></span>

<span data-ttu-id="755a2-186">Hozzáadhat egy alkalmazást az Azure AD-gyűjteményből, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="755a2-186">To add an application from the Azure AD Gallery, follow the steps below:</span></span>

1.  <span data-ttu-id="755a2-187">Nyissa meg a [Azure Portal](https://portal.azure.com) , és jelentkezzen be egy **globális rendszergazda** vagy **társadminisztrátor**.</span><span class="sxs-lookup"><span data-stu-id="755a2-187">Open the [Azure Portal](https://portal.azure.com) and sign in as a **Global Administrator** or **Co-admin**.</span></span>

2.  <span data-ttu-id="755a2-188">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="755a2-188">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="755a2-189">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="755a2-189">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="755a2-190">Kattintson a **vállalati alkalmazások** az Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="755a2-190">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="755a2-191">Kattintson a **Hozzáadás** gombra a jobb felső sarkában a **vállalati alkalmazások** panelen.</span><span class="sxs-lookup"><span data-stu-id="755a2-191">click the **Add** button at the top-right corner on the **Enterprise Applications** blade.</span></span>

6.  <span data-ttu-id="755a2-192">Kattintson a **nem-gyűjtemény alkalmazás.**</span><span class="sxs-lookup"><span data-stu-id="755a2-192">click **Non-gallery application.**</span></span>

7.  <span data-ttu-id="755a2-193">Adja meg az alkalmazás nevét a **neve** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="755a2-193">Enter the name of your application in the **Name** textbox.</span></span> <span data-ttu-id="755a2-194">Válassza ki **hozzáadása.**</span><span class="sxs-lookup"><span data-stu-id="755a2-194">Select **Add.**</span></span>

<span data-ttu-id="755a2-195">Egy rövid időszak után el az alkalmazás konfigurációs panelen láthatók.</span><span class="sxs-lookup"><span data-stu-id="755a2-195">After a short period, you be able to see the application’s configuration blade.</span></span>

### <a name="configure-the-application-for-password-single-sign-on"></a><span data-ttu-id="755a2-196">Állítsa be az alkalmazását jelszó egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="755a2-196">Configure the application for password single sign-on</span></span>

<span data-ttu-id="755a2-197">Egyszeri bejelentkezés egy alkalmazás konfigurálásához kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="755a2-197">To configure single sign-on for an application, follow the steps below:</span></span>

1.  <span data-ttu-id="755a2-198">Nyissa meg a [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**</span><span class="sxs-lookup"><span data-stu-id="755a2-198">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="755a2-199">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="755a2-199">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="755a2-200">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="755a2-200">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="755a2-201">Kattintson a **vállalati alkalmazások** az Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="755a2-201">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="755a2-202">Kattintson a **összes alkalmazás** az alkalmazások listájának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="755a2-202">click **All Applications** to view a list of all your applications.</span></span>

    1.  <span data-ttu-id="755a2-203">Ha azt szeretné, hogy itt megjelennek az alkalmazás nem látja, használja a **szűrő** vezérlő tetején a **összes alkalmazások listáját** és állítsa be a **megjelenítése** lehetőséggel **összes alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="755a2-203">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="755a2-204">Válassza ki az egyszeri bejelentkezés konfigurálni kívánt alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="755a2-204">Select the application you want to configure single sign-on.</span></span>

7.  <span data-ttu-id="755a2-205">Ha az alkalmazás betölt, kattintson a **egyszeri bejelentkezés** az alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="755a2-205">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="755a2-206">Válassza ki a módot **jelszóalapú bejelentkezés.**</span><span class="sxs-lookup"><span data-stu-id="755a2-206">Select the mode **Password-based Sign-on.**</span></span>

9.  <span data-ttu-id="755a2-207">Adja meg a **bejelentkezési URL-cím**.</span><span class="sxs-lookup"><span data-stu-id="755a2-207">Enter the **Sign-on URL**.</span></span> <span data-ttu-id="755a2-208">Ez az URL-CÍMÉT ahol adja meg a felhasználók a felhasználónevével és jelszavával bejelentkezni.</span><span class="sxs-lookup"><span data-stu-id="755a2-208">This is the URL where users enter their username and password to sign in to.</span></span> <span data-ttu-id="755a2-209">Győződjön meg arról, a mezők bejelentkezési URL-címen láthatók.</span><span class="sxs-lookup"><span data-stu-id="755a2-209">Ensure the sign in fields are visible at the URL.</span></span>

10. <span data-ttu-id="755a2-210">Felhasználók hozzárendelése az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="755a2-210">Assign users to the application.</span></span>

11. <span data-ttu-id="755a2-211">Emellett is megadhatja a felhasználó nevében hitelesítő adatok a felhasználók a sorok kijelöléséhez és parancsával **frissítéséhez szükséges hitelesítő adatokat** , majd gépelje be a felhasználónevet és jelszót a felhasználók nevében.</span><span class="sxs-lookup"><span data-stu-id="755a2-211">Additionally, you can also provide credentials on behalf of the user by selecting the rows of the users and clicking on **Update Credentials** and entering the username and password on behalf of the users.</span></span> <span data-ttu-id="755a2-212">Ellenkező esetben megkérdezi a felhasználókat a hitelesítő adatok magukat az indítás után.</span><span class="sxs-lookup"><span data-stu-id="755a2-212">Otherwise, users be prompted to enter the credentials themselves upon launch.</span></span>

## <a name="how-to-assign-a-user-to-an-application-directly"></a><span data-ttu-id="755a2-213">A felhasználó közvetlenül egy alkalmazás hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="755a2-213">How to assign a user to an application directly</span></span>

<span data-ttu-id="755a2-214">Hozzárendelése egy vagy több felhasználó alkalmazás közvetlenül, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="755a2-214">To assign one or more users to an application directly, follow the steps below:</span></span>

1.  <span data-ttu-id="755a2-215">Nyissa meg a [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazdája.**</span><span class="sxs-lookup"><span data-stu-id="755a2-215">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator.**</span></span>

2.  <span data-ttu-id="755a2-216">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="755a2-216">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="755a2-217">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="755a2-217">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="755a2-218">Kattintson a **vállalati alkalmazások** az Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="755a2-218">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="755a2-219">Kattintson a **összes alkalmazás** az alkalmazások listájának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="755a2-219">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="755a2-220">Ha azt szeretné, hogy itt megjelennek az alkalmazás nem látja, használja a **szűrő** vezérlő tetején a **összes alkalmazások listáját** és állítsa be a **megjelenítése** lehetőséggel **összes alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="755a2-220">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="755a2-221">Válassza ki szeretné osztani a felhasználót, hogy a listában az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="755a2-221">Select the application you want to assign a user to from the list.</span></span>

7.  <span data-ttu-id="755a2-222">Ha az alkalmazás betölt, kattintson **felhasználók és csoportok** az alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="755a2-222">Once the application loads, click **Users and Groups** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="755a2-223">Kattintson a **Hozzáadás** gombra kattint, a a **felhasználók és csoportok** nyissa meg a listában a **hozzáadása hozzárendelés** panelen.</span><span class="sxs-lookup"><span data-stu-id="755a2-223">Click the **Add** button on top of the **Users and Groups** list to open the **Add Assignment** blade.</span></span>

9.  <span data-ttu-id="755a2-224">Kattintson a **felhasználók és csoportok** a választó a **hozzáadása hozzárendelés** panelen.</span><span class="sxs-lookup"><span data-stu-id="755a2-224">click the **Users and groups** selector from the **Add Assignment** blade.</span></span>

10. <span data-ttu-id="755a2-225">Írja be a **teljes név** vagy **e-mail cím** érdekli hozzárendelése a felhasználó a **Keresés név vagy e-mail cím alapján** keresőmezőbe.</span><span class="sxs-lookup"><span data-stu-id="755a2-225">Type in the **full name** or **email address** of the user you are interested in assigning into the **Search by name or email address** search box.</span></span>

11. <span data-ttu-id="755a2-226">Vigye a **felhasználói** a listában, hogy láthatóvá váljon a **jelölőnégyzet**.</span><span class="sxs-lookup"><span data-stu-id="755a2-226">Hover over the **user** in the list to reveal a **checkbox**.</span></span> <span data-ttu-id="755a2-227">A felhasználói profil fénykép vagy adja hozzá a felhasználót emblémát jelölőnégyzetét, kattintson a **kijelölt** listája.</span><span class="sxs-lookup"><span data-stu-id="755a2-227">Click the checkbox next to the user’s profile photo or logo to add your user to the **Selected** list.</span></span>

12. <span data-ttu-id="755a2-228">**Választható lehetőség:** Ha azt szeretné, hogy **egynél több felhasználó hozzáadása**, egy másik típus **teljes név** vagy **e-mail cím** be a **Keresés név vagy e-mail cím alapján** mező, és a jelölőnégyzet bejelölésével adja hozzá a felhasználót, hogy a **kijelölt** listája.</span><span class="sxs-lookup"><span data-stu-id="755a2-228">**Optional:** If you would like to **add more than one user**, type in another **full name** or **email address** into the **Search by name or email address** search box, and click the checkbox to add this user to the **Selected** list.</span></span>

13. <span data-ttu-id="755a2-229">Ha elkészült, válassza a felhasználók, kattintson a **válasszon** gombra kattintva vegye fel a listára a felhasználók és csoportok hozzá kell rendelni az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="755a2-229">When you are finished selecting users, click the **Select** button to add them to the list of users and groups to be assigned to the application.</span></span>

14. <span data-ttu-id="755a2-230">**Választható lehetőség:** kattintson a **Szerepkörválasztás** a választó a **hozzáadása hozzárendelés** hozzárendelése a kiválasztott felhasználói szerepkör kiválasztása panel.</span><span class="sxs-lookup"><span data-stu-id="755a2-230">**Optional:** click the **Select Role** selector in the **Add Assignment** blade to select a role to assign to the users you have selected.</span></span>

15. <span data-ttu-id="755a2-231">Kattintson a **hozzárendelése** gombra kattintva a kijelölt felhasználók az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="755a2-231">Click the **Assign** button to assign the application to the selected users.</span></span>

<span data-ttu-id="755a2-232">Rövid ideig a kijelölt felhasználók tudják elindítani ezeket az alkalmazásokat a hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="755a2-232">After a short period, the users you have selected be able to launch these applications in the Access Panel.</span></span>

## <a name="if-these-troubleshooting-steps-do-not-the-resolve-the-issue"></a><span data-ttu-id="755a2-233">Ha ezek a hibaelhárítási lépéseket nem a hárítsa el a problémát.</span><span class="sxs-lookup"><span data-stu-id="755a2-233">If these troubleshooting steps do not the resolve the issue.</span></span> 

<span data-ttu-id="755a2-234">támogatási jegy megnyitása a következő információkat, ha rendelkezésre áll:</span><span class="sxs-lookup"><span data-stu-id="755a2-234">open a support ticket with the following information if available:</span></span>

-   <span data-ttu-id="755a2-235">Megfelelési hiba azonosítója</span><span class="sxs-lookup"><span data-stu-id="755a2-235">Correlation error ID</span></span>

-   <span data-ttu-id="755a2-236">Egyszerű felhasználónév (felhasználó e-mail címe)</span><span class="sxs-lookup"><span data-stu-id="755a2-236">UPN (user email address)</span></span>

-   <span data-ttu-id="755a2-237">A TenantID</span><span class="sxs-lookup"><span data-stu-id="755a2-237">TenantID</span></span>

-   <span data-ttu-id="755a2-238">Böngésző típusa</span><span class="sxs-lookup"><span data-stu-id="755a2-238">Browser type</span></span>

-   <span data-ttu-id="755a2-239">Időzóna és idő/időkeretre során hiba történik.</span><span class="sxs-lookup"><span data-stu-id="755a2-239">Time zone and time/timeframe during error occurs</span></span>

-   <span data-ttu-id="755a2-240">Fiddler nyomkövetések</span><span class="sxs-lookup"><span data-stu-id="755a2-240">Fiddler traces</span></span>

## <a name="next-steps"></a><span data-ttu-id="755a2-241">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="755a2-241">Next steps</span></span>
[<span data-ttu-id="755a2-242">Adja meg az egyszeri bejelentkezés az alkalmazásokba a Proxy</span><span class="sxs-lookup"><span data-stu-id="755a2-242">Provide single sign-on to your apps with Application Proxy</span></span>](active-directory-application-proxy-sso-using-kcd.md)
