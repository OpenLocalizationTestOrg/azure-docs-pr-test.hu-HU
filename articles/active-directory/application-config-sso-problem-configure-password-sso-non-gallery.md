---
title: "A probléma jelszó egyszeri bejelentkezés nem galéria alkalmazások konfigurálása |} Microsoft Docs"
description: "A gyakori problémák személyek arcfelismerési áttekinteni jelszó egyszeri bejelentkezéshez, amelyek nem szerepelnek az Azure AD Application Gallery egyéni nem galéria-alkalmazások konfigurálása"
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
ms.openlocfilehash: 9c76b6f3495e2dd759a156fcef97b57aece8d632
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="problem-configuring-password-single-sign-on-for-a-non-gallery-application"></a><span data-ttu-id="51fe4-103">A probléma jelszó egyszeri bejelentkezés nem galéria alkalmazások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="51fe4-103">Problem configuring password single sign-on for a non-gallery application</span></span>

<span data-ttu-id="51fe4-104">Ez a cikk segítenek megérteni a gyakori problémák személyek arcfelismerési konfigurálásakor **jelszó egyszeri bejelentkezés** nem galéria alkalmazással.</span><span class="sxs-lookup"><span data-stu-id="51fe4-104">This article help you to understand the common problems people face when configuring **Password single sign-on** with a non-gallery application.</span></span>

## <a name="how-to-capture-sign-in-fields-for-an-application"></a><span data-ttu-id="51fe4-105">Az alkalmazás bejelentkezési mezők rögzítése</span><span class="sxs-lookup"><span data-stu-id="51fe4-105">How to capture sign-in fields for an application</span></span>

<span data-ttu-id="51fe4-106">Bejelentkezési mező rögzítési csak a támogatott bejelentkezési lap HTML-kompatibilis, és **nem támogatott a nem szabványos bejelentkezési lapok**, például a Flash, vagy egyéb nem HTML-kompatibilis technológiákat használó.</span><span class="sxs-lookup"><span data-stu-id="51fe4-106">Sign-in field capture is only supported for HTML-enabled sign-in pages and is **not supported for non-standard sign-in pages**, like those that use Flash, or other non-HTML-enabled technologies.</span></span>

<span data-ttu-id="51fe4-107">Az egyéni alkalmazások rögzítheti a bejelentkezési mező két módja van:</span><span class="sxs-lookup"><span data-stu-id="51fe4-107">There are two ways you can capture sign-in fields for your custom applications:</span></span>

-   <span data-ttu-id="51fe4-108">Automatikus bejelentkezés mező rögzítése</span><span class="sxs-lookup"><span data-stu-id="51fe4-108">Automatic sign-in field capture</span></span>

-   <span data-ttu-id="51fe4-109">Manuális bejelentkezési mező rögzítése</span><span class="sxs-lookup"><span data-stu-id="51fe4-109">Manual sign-in field capture</span></span>

<span data-ttu-id="51fe4-110">**Automatikus bejelentkezés mező rögzítési** jól működik a legtöbb HTML-kompatibilis bejelentkezési lapok, ha azok **jól ismert DIV azonosítóit a felhasználónevet és jelszót adjon meg** mező.</span><span class="sxs-lookup"><span data-stu-id="51fe4-110">**Automatic sign-in field capture** works well with most HTML-enabled sign-in pages, if they use **well-known DIV IDs for the username and password input** field.</span></span> <span data-ttu-id="51fe4-111">A működés módja a HTML-KÓDBAN a lapon található a feltételeknek megfelelő DIV azonosítók lekaparást és azt a jelszavakat, később is visszajátszásos, majd mentse a, hogy az alkalmazás metaadatai.</span><span class="sxs-lookup"><span data-stu-id="51fe4-111">The way this works is by scraping the HTML on the page to find DIV IDs that match certain criteria and by then saving that metadata for this application so we can replay passwords to it later.</span></span>

<span data-ttu-id="51fe4-112">**Manuális bejelentkezési mező rögzítési** abban az esetben is használható, amely az alkalmazás **szállító nem címke** a bejelentkezéshez használt beviteli mezők.</span><span class="sxs-lookup"><span data-stu-id="51fe4-112">**Manual sign-in field capture** can be used in the case that the application **vendor does not label** the input fields used for sign in.</span></span> <span data-ttu-id="51fe4-113">Manuális bejelentkezési mező rögzítési is használható abban az esetben, ha a **szállító Renderelés több mező** lehet, amely automatikusan észleli.</span><span class="sxs-lookup"><span data-stu-id="51fe4-113">Manual sign-in field capture can also be used in the case when the **vendor renders multiple fields** which cannot be auto-detected.</span></span> <span data-ttu-id="51fe4-114">Az Azure AD tárolhatja az adatokat, a bejelentkezési oldal, a lehető legtöbb mezők mindaddig, amíg Ön mondja el, ha ezen mezők szerepelnek a lap.</span><span class="sxs-lookup"><span data-stu-id="51fe4-114">Azure AD can store data for as many fields as are on the sign in page, so long as you tell us where those fields are on the page.</span></span>

<span data-ttu-id="51fe4-115">Általában **automatikus bejelentkezési mező rögzítése nem működik, ha mindig javasoljuk, hogy a manuális beállítás közben.**</span><span class="sxs-lookup"><span data-stu-id="51fe4-115">In general, **if automatic sign-in field capture does not work, we always suggest trying the manual option.**</span></span>

### <a name="how-to-automatically-capture-sign-in-fields-for-an-application"></a><span data-ttu-id="51fe4-116">Az alkalmazás bejelentkezési mezők automatikusan rögzítése</span><span class="sxs-lookup"><span data-stu-id="51fe4-116">How to automatically capture sign-in fields for an application</span></span>

<span data-ttu-id="51fe4-117">Konfigurálása **jelszó-alapú egyszeri bejelentkezést** egy alkalmazást, amely a **automatikus bejelentkezési mező rögzítési**, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="51fe4-117">To configure **Password-based Single Sign-on** for an application using **automatic sign-in field capture**, follow the steps below:</span></span>

1.  <span data-ttu-id="51fe4-118">Nyissa meg a [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**</span><span class="sxs-lookup"><span data-stu-id="51fe4-118">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="51fe4-119">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="51fe4-119">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="51fe4-120">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="51fe4-120">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="51fe4-121">Kattintson a **vállalati alkalmazások** az Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="51fe4-121">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="51fe4-122">Kattintson a **összes alkalmazás** az alkalmazások listájának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="51fe4-122">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="51fe4-123">Ha azt szeretné, hogy itt megjelennek az alkalmazás nem látja, használja a **szűrő** vezérlő tetején a **összes alkalmazások listáját** és állítsa be a **megjelenítése** lehetőséggel **összes alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="51fe4-123">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="51fe4-124">Válassza ki az egyszeri bejelentkezés konfigurálni kívánt alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="51fe4-124">Select the application you want to configure single sign-on.</span></span>

7.  <span data-ttu-id="51fe4-125">Ha az alkalmazás betölt, kattintson a **egyszeri bejelentkezés** az alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="51fe4-125">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="51fe4-126">Válassza ki a módot **jelszóalapú bejelentkezés.**</span><span class="sxs-lookup"><span data-stu-id="51fe4-126">Select the mode **Password-based Sign-on.**</span></span>

9.  <span data-ttu-id="51fe4-127">Adja meg a **bejelentkezési URL-cím**.</span><span class="sxs-lookup"><span data-stu-id="51fe4-127">Enter the **Sign-on URL**.</span></span> <span data-ttu-id="51fe4-128">Ez az URL-CÍMÉT ahol adja meg a felhasználók a felhasználónevével és jelszavával bejelentkezni.</span><span class="sxs-lookup"><span data-stu-id="51fe4-128">This is the URL where users enter their username and password to sign in to.</span></span> <span data-ttu-id="51fe4-129">**Biztosítsa, hogy a bejelentkezési mező látható, akkor adja meg a megadott URL-címen**.</span><span class="sxs-lookup"><span data-stu-id="51fe4-129">**Ensure the sign in fields are visible at the URL you provide**.</span></span>

10. <span data-ttu-id="51fe4-130">Kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="51fe4-130">Click the **Save** button.</span></span>

11. <span data-ttu-id="51fe4-131">Ha így tesz, azt fogja automatikusan scrape a felhasználónévhez URL-címet, és jelszó beviteli mező, és biztonságos átvitelére a jelszavakat, hogy a hozzáférési panel bővítmény használó alkalmazások az Azure AD használatára.</span><span class="sxs-lookup"><span data-stu-id="51fe4-131">Once you do that, we’ll automatically scrape that URL for a username and password input box and allow you to use Azure AD to securely transmit passwords to that application using the access panel browser extension.</span></span>

## <a name="how-to-manually-capture-sign-in-fields-for-an-application"></a><span data-ttu-id="51fe4-132">Manuálisan rögzítése az alkalmazás bejelentkezési mezők</span><span class="sxs-lookup"><span data-stu-id="51fe4-132">How to manually capture sign-in fields for an application</span></span>

<span data-ttu-id="51fe4-133">Bejelentkezési mezőket manuálisan rögzítéséhez kell a hozzáférési Panel bővítmény telepítve és **nem fut az InPrivate-, incognito vagy privát módban.**</span><span class="sxs-lookup"><span data-stu-id="51fe4-133">To manually capture sign in fields, you must first have the Access Panel Browser extension installed and **not be running in inPrivate, incognito, or private mode.**</span></span> <span data-ttu-id="51fe4-134">A bővítmény telepítéséhez kövesse a [a hozzáférési Panel bővítmény telepítése](#i-cannot-manually-detect-sign-in-fields-for-my-application) szakasz.</span><span class="sxs-lookup"><span data-stu-id="51fe4-134">To install the browser extension, follow the steps in the [How to install the Access Panel Browser extension](#i-cannot-manually-detect-sign-in-fields-for-my-application) section.</span></span>

<span data-ttu-id="51fe4-135">Konfigurálása **jelszó-alapú egyszeri bejelentkezést** egy alkalmazást, amely a **manuális bejelentkezési mező rögzítési**, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="51fe4-135">To configure **Password-based Single Sign-on** for an application using **manual sign-in field capture**, follow the steps below:</span></span>

1.  <span data-ttu-id="51fe4-136">Nyissa meg a [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**</span><span class="sxs-lookup"><span data-stu-id="51fe4-136">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="51fe4-137">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="51fe4-137">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="51fe4-138">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="51fe4-138">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="51fe4-139">Kattintson a **vállalati alkalmazások** az Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="51fe4-139">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="51fe4-140">Kattintson a **összes alkalmazás** az alkalmazások listájának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="51fe4-140">click **All Applications** to view a list of all your applications.</span></span>

   * <span data-ttu-id="51fe4-141">Ha azt szeretné, hogy itt megjelennek az alkalmazás nem látja, használja a **szűrő** vezérlő tetején a **összes alkalmazások listáját** és állítsa be a **megjelenítése** lehetőséggel **összes alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="51fe4-141">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="51fe4-142">Válassza ki az egyszeri bejelentkezés konfigurálni kívánt alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="51fe4-142">Select the application you want to configure single sign-on.</span></span>

7.  <span data-ttu-id="51fe4-143">Ha az alkalmazás betölt, kattintson a **egyszeri bejelentkezés** az alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="51fe4-143">Once the application loads, click the **Single sign-on** from the application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="51fe4-144">Válassza ki a módot **jelszóalapú bejelentkezés.**</span><span class="sxs-lookup"><span data-stu-id="51fe4-144">Select the mode **Password-based Sign-on.**</span></span>

9.  <span data-ttu-id="51fe4-145">Adja meg a **bejelentkezési URL-cím**.</span><span class="sxs-lookup"><span data-stu-id="51fe4-145">Enter the **Sign-on URL**.</span></span> <span data-ttu-id="51fe4-146">Ez az URL-CÍMÉT ahol adja meg a felhasználók a felhasználónevével és jelszavával bejelentkezni.</span><span class="sxs-lookup"><span data-stu-id="51fe4-146">This is the URL where users enter their username and password to sign in to.</span></span> <span data-ttu-id="51fe4-147">**Biztosítsa, hogy a bejelentkezési mező látható, akkor adja meg a megadott URL-címen**.</span><span class="sxs-lookup"><span data-stu-id="51fe4-147">**Ensure the sign in fields are visible at the URL you provide**.</span></span>

10. <span data-ttu-id="51fe4-148">Kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="51fe4-148">Click the **Save** button.</span></span>

11. <span data-ttu-id="51fe4-149">Ha így tesz, azt fogja automatikusan scrape a felhasználónévhez URL-címet, és jelszó beviteli mező, és biztonságos átvitelére a jelszavakat, hogy a hozzáférési panel bővítmény használó alkalmazások az Azure AD használatára.</span><span class="sxs-lookup"><span data-stu-id="51fe4-149">Once you do that, we’ll automatically scrape that URL for a username and password input box and allow you to use Azure AD to securely transmit passwords to that application using the access panel browser extension.</span></span> <span data-ttu-id="51fe4-150">Abban az esetben, ez nem sikerül, akkor **állítsa át a bejelentkezési módot manuális bejelentkezési mező rögzíti** által folytatni szeretné a 12. lépés.</span><span class="sxs-lookup"><span data-stu-id="51fe4-150">In the case that this fails, you can **change the sign-in mode to use manual sign-in field capture** by continuing to step 12.</span></span>

12. <span data-ttu-id="51fe4-151">Kattintson a **konfigurálása &lt;appname&gt; jelszó egyszeri bejelentkezési beállítások**.</span><span class="sxs-lookup"><span data-stu-id="51fe4-151">click **Configure &lt;appname&gt; Password Single Sign-on Settings**.</span></span>

13. <span data-ttu-id="51fe4-152">Válassza ki a **manuálisan észleli a bejelentkezési mező** konfigurációs beállítást.</span><span class="sxs-lookup"><span data-stu-id="51fe4-152">Select the **Manually detect sign-in fields** configuration option.</span></span>

14. <span data-ttu-id="51fe4-153">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="51fe4-153">Click **Ok**.</span></span>

15. <span data-ttu-id="51fe4-154">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="51fe4-154">Click **Save**.</span></span>

16. <span data-ttu-id="51fe4-155">Kövesse a képernyőn a hozzáférési Panel használatára.</span><span class="sxs-lookup"><span data-stu-id="51fe4-155">Follow the on screen instructions to use the Access Panel.</span></span>

## <a name="i-see-a-we-couldnt-find-any-sign-in-fields-at-that-url-error"></a><span data-ttu-id="51fe4-156">A "Minden bejelentkezés mezőt, hogy az URL-címen nem található" hiba</span><span class="sxs-lookup"><span data-stu-id="51fe4-156">I see a “We couldn’t find any sign-in fields at that URL” error</span></span>

<span data-ttu-id="51fe4-157">Ezt a hibaüzenetet látja, ha a bejelentkezési mezők automatikus észlelése nem sikerült.</span><span class="sxs-lookup"><span data-stu-id="51fe4-157">You see this error when automatic detection of sign-in fields fails.</span></span> <span data-ttu-id="51fe4-158">A probléma megoldásához próbálja meg manuális bejelentkezési mező észlelési lépéseit követve a [manuálisan rögzítése az alkalmazás bejelentkezési mezők](#how-to-manually-capture-sign-in-fields-for-an-application) szakasz.</span><span class="sxs-lookup"><span data-stu-id="51fe4-158">To resolve this issue, try manual sign-in field detection by following the steps in the [How to manually capture sign-in fields for an application](#how-to-manually-capture-sign-in-fields-for-an-application) section.</span></span>

## <a name="i-see-an-unable-to-save-single-sign-on-configuration-error"></a><span data-ttu-id="51fe4-159">"Egyszeri bejelentkezést a konfiguráció mentéséhez, nem" látható hiba</span><span class="sxs-lookup"><span data-stu-id="51fe4-159">I see an “Unable to save Single Sign-on configuration” error</span></span>

<span data-ttu-id="51fe4-160">Ritka esetekben az egyszeri bejelentkezés beállításainak frissítése sikertelen lehet.</span><span class="sxs-lookup"><span data-stu-id="51fe4-160">In certain rare cases, updating the single sign-on configuration can fail.</span></span> <span data-ttu-id="51fe4-161">Hárítsa el a próbálja az egyszeri bejelentkezés konfigurációjának mentése újra.</span><span class="sxs-lookup"><span data-stu-id="51fe4-161">TO resolve this try saving the single sign-on configuration again.</span></span>

<span data-ttu-id="51fe4-162">Ha ez továbbra is egységesen sikertelen, nyissa meg a támogatási esetet, és adja meg az összegyűjtött adatokat a [a portál értesítései részleteinek megtekintése](#i-cannot-manually-detect-sign-in-fields-for-my-application) és [segítség kérése értesítési részletek küld egy támogatási szakértőhöz](#how-to-get-help-by-sending-notification-details-to-a-support-engineer) szakaszok.</span><span class="sxs-lookup"><span data-stu-id="51fe4-162">If this continues to fail consistently, open a support case and provide the information gathered in the [How to see the details of a portal notification](#i-cannot-manually-detect-sign-in-fields-for-my-application) and [How to get help by sending notification details to a support engineer](#how-to-get-help-by-sending-notification-details-to-a-support-engineer) sections.</span></span>

## <a name="i-cannot-manually-detect-sign-in-fields-for-my-application"></a><span data-ttu-id="51fe4-163">I nem manuálisan észlel bejelentkezési mezőket az alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="51fe4-163">I cannot manually detect sign in fields for my application</span></span>

<span data-ttu-id="51fe4-164">Láthatja, ha manuális észlelése nem működik az viselkedésmódok közé:</span><span class="sxs-lookup"><span data-stu-id="51fe4-164">Some of the behaviors you might see when manual detection is not working include:</span></span>

-   <span data-ttu-id="51fe4-165">A manuális rögzítési folyamat működéséhez jelent meg, de a rögzített mezők nem volt megfelelő</span><span class="sxs-lookup"><span data-stu-id="51fe4-165">The manual capture process appeared to work, but the fields captured were not correct</span></span>

-   <span data-ttu-id="51fe4-166">A jobb oldali mezők nem beolvasni a kijelölt a rögzítési folyamat végrehajtása során</span><span class="sxs-lookup"><span data-stu-id="51fe4-166">The right fields don’t get highlighted when performing the capture process</span></span>

-   <span data-ttu-id="51fe4-167">A rögzítési folyamat veszi át me az alkalmazás bejelentkezési oldalt, a várt, de semmi nem történik</span><span class="sxs-lookup"><span data-stu-id="51fe4-167">The capture process takes me to the application’s login page as expected, but nothing happens</span></span>

-   <span data-ttu-id="51fe4-168">Kézi rögzítési úgy tűnik, hogy működik, de az SSO nem fordulhat elő, ha a hozzáférési panelen igényelheti az alkalmazás a felhasználók keresse meg.</span><span class="sxs-lookup"><span data-stu-id="51fe4-168">Manual capture appears to work, but SSO doesn’t happen when my users navigate to the application from the Access Panel.</span></span>

<span data-ttu-id="51fe4-169">Ellenőrizze az alábbiakat, ha ezeket a problémákat tapasztal:</span><span class="sxs-lookup"><span data-stu-id="51fe4-169">check the following if you encounter any of these issues:</span></span>

-   <span data-ttu-id="51fe4-170">Győződjön meg arról, hogy rendelkezik-e a hozzáférési panel bővítmény legújabb verziója **telepítve** és **engedélyezett** a lépések a [a hozzáférési Panel bővítmény telepítése](#how-to-install-the-access-panel-browser-extension) szakasz.</span><span class="sxs-lookup"><span data-stu-id="51fe4-170">Ensure that you have the latest version of the access panel browser extension **installed** and **enabled** by following the steps in the [How to install the Access Panel Browser extension](#how-to-install-the-access-panel-browser-extension) section.</span></span>

-   <span data-ttu-id="51fe4-171">Győződjön meg arról, hogy nem kívánt a rögzítési folyamat közben a böngésző a **incognito, inPrivate vagy privát mód**.</span><span class="sxs-lookup"><span data-stu-id="51fe4-171">Ensure that you are not attempting the capture process while your browser in **incognito, inPrivate, or Private mode**.</span></span> <span data-ttu-id="51fe4-172">A hozzáférési panel bővítmény e mód nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="51fe4-172">The access panel extension is not supported in these modes.</span></span>

-   <span data-ttu-id="51fe4-173">Győződjön meg arról, hogy a felhasználók nem próbál bejelentkezni az alkalmazás a hozzáférési panelen igényelheti közben a **incognito, inPrivate vagy privát mód**.</span><span class="sxs-lookup"><span data-stu-id="51fe4-173">Ensure that your users are not trying to sign in to the application from the access panel while in **incognito, inPrivate, or Private mode**.</span></span> <span data-ttu-id="51fe4-174">A hozzáférési panel bővítmény e mód nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="51fe4-174">The access panel extension is not supported in these modes.</span></span>

-   <span data-ttu-id="51fe4-175">Próbálja meg a manuális rögzítési folyamat ismét, annak biztosítása, a vörös jelölőket keresztül a megfelelő mezőket.</span><span class="sxs-lookup"><span data-stu-id="51fe4-175">Try the manual capture process again, ensuring the red markers are over the correct fields.</span></span>

-   <span data-ttu-id="51fe4-176">Ha a kézi rögzítési folyamat úgy tűnik, hogy leállását, vagy a bejelentkezési oldal nem semmit (a fenti 3. eset), ismételje meg a manuális rögzítési folyamat.</span><span class="sxs-lookup"><span data-stu-id="51fe4-176">If the manual capture process seems to hang, or the sign in page doesn’t do anything (case 3 above), try the manual capture process again.</span></span> <span data-ttu-id="51fe4-177">De ezúttal a folyamat befejezése után nyomja le az **F12** gombra kattintva nyissa meg a böngésző fejlesztői konzolján.</span><span class="sxs-lookup"><span data-stu-id="51fe4-177">But, this time after completing the process, press the **F12** button to open your browser’s developer console.</span></span> <span data-ttu-id="51fe4-178">Egyszer, nyissa meg a **konzol** és írja be **window.location= "&lt;a az alkalmazás konfigurálásakor megadott URL-címet adja meg a bejelentkezési&gt;"** , és nyomja le az **Enter**.</span><span class="sxs-lookup"><span data-stu-id="51fe4-178">Once there, open the **console** and type **window.location=”&lt;enter the sign in url you specified when configuring the app&gt;”** and then press **Enter**.</span></span> <span data-ttu-id="51fe4-179">A kényszerített egy lap átirányítási, amely a rögzítési folyamat befejezéséhez, és tárolja a mezőket, amelyeknek már rögzített.</span><span class="sxs-lookup"><span data-stu-id="51fe4-179">This force a page redirect which end the capture process and store the fields that have been captured.</span></span>

<span data-ttu-id="51fe4-180">Ha ezek egyike sem működik, meg, segítünk.</span><span class="sxs-lookup"><span data-stu-id="51fe4-180">If none of these approaches work for you, we can help.</span></span> <span data-ttu-id="51fe4-181">Nyisson meg egy támogatási esetet mi próbált, valamint begyűjtött információ részleteit a [a portál értesítései részleteinek megtekintése](#i-cannot-manually-detect-sign-in-fields-for-my-application) és [segítség kérése értesítési részletek küld egy támogatási szakértőhöz](#how-to-get-help-by-sending-notification-details-to-a-support-engineer) részek (ha van ilyen).</span><span class="sxs-lookup"><span data-stu-id="51fe4-181">Open a support case with the details of what you tried, as well as the information gathered in the [How to see the details of a portal notification](#i-cannot-manually-detect-sign-in-fields-for-my-application) and [How to get help by sending notification details to a support engineer](#how-to-get-help-by-sending-notification-details-to-a-support-engineer) sections (if applicable).</span></span>

## <a name="how-to-install-the-access-panel-browser-extension"></a><span data-ttu-id="51fe4-182">A hozzáférési Panel bővítmény telepítése</span><span class="sxs-lookup"><span data-stu-id="51fe4-182">How to install the Access Panel Browser extension</span></span>

<span data-ttu-id="51fe4-183">A hozzáférési Panel bővítmény telepítéséhez kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="51fe4-183">To install the Access Panel Browser extension, follow the steps below:</span></span>

1.  <span data-ttu-id="51fe4-184">Nyissa meg a [hozzáférési Panel](https://myapps.microsoft.com) valamelyik támogatott böngészők és való bejelentkezést egy **felhasználói** az Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="51fe4-184">Open the [Access Panel](https://myapps.microsoft.com) in one of the supported browsers and sign in as a **user** in your Azure AD.</span></span>

2.  <span data-ttu-id="51fe4-185">Kattintson egy **jelszó-SSO alkalmazás** a hozzáférési panelen.</span><span class="sxs-lookup"><span data-stu-id="51fe4-185">click a **password-SSO application** in the Access Panel.</span></span>

3.  <span data-ttu-id="51fe4-186">Válassza ki a szoftver telepítéséhez megkérdezi **telepítés**.</span><span class="sxs-lookup"><span data-stu-id="51fe4-186">In the prompt asking to install the software, select **Install Now**.</span></span>

4.  <span data-ttu-id="51fe4-187">A böngésző alapján kell irányítani a letöltési hivatkozás.</span><span class="sxs-lookup"><span data-stu-id="51fe4-187">Based on your browser you be directed to the download link.</span></span> <span data-ttu-id="51fe4-188">**Adja hozzá** az bővítményt, hogy a böngészőben.</span><span class="sxs-lookup"><span data-stu-id="51fe4-188">**Add** the extension to your browser.</span></span>

5.  <span data-ttu-id="51fe4-189">Ha a böngésző, válassza ki vagy **engedélyezése** vagy **engedélyezése** a bővítményt.</span><span class="sxs-lookup"><span data-stu-id="51fe4-189">If your browser asks, select to either **Enable** or **Allow** the extension.</span></span>

6.  <span data-ttu-id="51fe4-190">Korábban telepítve, **indítsa újra a** a böngésző-munkamenetet.</span><span class="sxs-lookup"><span data-stu-id="51fe4-190">Once installed, **restart** your browser session.</span></span>

7.  <span data-ttu-id="51fe4-191">Jelentkezzen be a hozzáférési panelre, és tekintse meg, ha **indítása** a jelszó-egyszeri bejelentkezés alkalmazásokhoz.</span><span class="sxs-lookup"><span data-stu-id="51fe4-191">Sign in into the Access Panel and see if you can **launch** your password-SSO applications.</span></span>

<span data-ttu-id="51fe4-192">Az alábbi közvetlen hivatkozások közül a Chrome és Firefox is letöltheti a bővítmény:</span><span class="sxs-lookup"><span data-stu-id="51fe4-192">You may also download the extension for Chrome and Firefox from the direct links below:</span></span>

-   [<span data-ttu-id="51fe4-193">Chrome hozzáférési Panel bővítmény</span><span class="sxs-lookup"><span data-stu-id="51fe4-193">Chrome Access Panel Extension</span></span>](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

-   [<span data-ttu-id="51fe4-194">A Firefox hozzáférési Panel bővítmény</span><span class="sxs-lookup"><span data-stu-id="51fe4-194">Firefox Access Panel Extension</span></span>](https://addons.mozilla.org/firefox/addon/access-panel-extension/)

## <a name="how-to-see-the-details-of-a-portal-notification"></a><span data-ttu-id="51fe4-195">A portál értesítései részleteinek megtekintése</span><span class="sxs-lookup"><span data-stu-id="51fe4-195">How to see the details of a portal notification</span></span>

<span data-ttu-id="51fe4-196">A portál értesítései részleteit láthatja az alábbi lépéseket követve:</span><span class="sxs-lookup"><span data-stu-id="51fe4-196">You can see the details of any portal notification by following the steps below:</span></span>

1.  <span data-ttu-id="51fe4-197">Kattintson a **értesítések** (a harang) ikonra a képernyő jobb felső sarkában az Azure portálon</span><span class="sxs-lookup"><span data-stu-id="51fe4-197">click the **Notifications** icon (the bell) in the upper right of the Azure Portal</span></span>

2.  <span data-ttu-id="51fe4-198">Válassza ki az értesítésekhez egy **hiba** állapota (amelyek a piros (!) mellett).</span><span class="sxs-lookup"><span data-stu-id="51fe4-198">Select any notification in an **Error** state (those with a red (!) next to them).</span></span>

  ><span data-ttu-id="51fe4-199">! Vegye figyelembe], nem kattintson az értesítések egy **sikeres** vagy **folyamatban lévő** állapotát.</span><span class="sxs-lookup"><span data-stu-id="51fe4-199">!NOTE] You cannot click notifications in a **Successful** or **In Progress** state.</span></span>
  >
  >

3.  <span data-ttu-id="51fe4-200">A Megnyitás a **értesítési részletek** panelen.</span><span class="sxs-lookup"><span data-stu-id="51fe4-200">This open the **Notification Details** blade.</span></span>

4.  <span data-ttu-id="51fe4-201">Ez a témakör a problémával kapcsolatos további részletekért megértéséhez.</span><span class="sxs-lookup"><span data-stu-id="51fe4-201">Use this information yourself to understand more details about the problem.</span></span>

5.  <span data-ttu-id="51fe4-202">Ha további segítségre van, a ezek az információk megosztása a támogatási szakember vagy a csoport segítség a probléma megoldásában.</span><span class="sxs-lookup"><span data-stu-id="51fe4-202">If you still need help, you can also share this information with a support engineer or the product group to get help with your problem.</span></span>

6.  <span data-ttu-id="51fe4-203">Kattintson a **másolási** **ikon** jobb oldalán a **hiba másolása** szövegmező megosztani a támogatási szolgálathoz vagy a termék csoport mérnöke értesítés részleteinek másolása.</span><span class="sxs-lookup"><span data-stu-id="51fe4-203">Click the **copy** **icon** to the right of the **Copy error** textbox to copy all the notification details to share with a support or product group engineer.</span></span>

## <a name="how-to-get-help-by-sending-notification-details-to-a-support-engineer"></a><span data-ttu-id="51fe4-204">Segítség kérése értesítési részletek küld egy támogatási szakértőhöz</span><span class="sxs-lookup"><span data-stu-id="51fe4-204">How to get help by sending notification details to a support engineer</span></span>

<span data-ttu-id="51fe4-205">Nagyon fontos, hogy megosztott **alább felsorolt összes részletes** , ha segítségre van szüksége, így gyorsan segít a támogatási szakértőhöz.</span><span class="sxs-lookup"><span data-stu-id="51fe4-205">It is very important that you share **all the details listed below** with a support engineer if you need help, so that they can help you quickly.</span></span> <span data-ttu-id="51fe4-206">Ehhez az egyszerű **elkészíti a képernyőfelvételt** vagy kattintson a **másolási hibaikon**, míg a talált jobb oldalán a **hiba másolása** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="51fe4-206">You can do this easily by **taking a screenshot,** or by clicking the **Copy error icon**, found to the right of the **Copy error** textbox.</span></span>

## <a name="notification-details-explained"></a><span data-ttu-id="51fe4-207">Értesítési részletek alapján</span><span class="sxs-lookup"><span data-stu-id="51fe4-207">Notification Details Explained</span></span>

<span data-ttu-id="51fe4-208">Az alábbiakban azt ismerteti, több milyen az értesítés azt jelenti, hogy elemeket, és azok példákat.</span><span class="sxs-lookup"><span data-stu-id="51fe4-208">The below explains more what each of the notification items means, and gives examples of each of them.</span></span>

### <a name="essential-notification-items"></a><span data-ttu-id="51fe4-209">Alapvető értesítési elemek</span><span class="sxs-lookup"><span data-stu-id="51fe4-209">Essential Notification Items</span></span>

-   <span data-ttu-id="51fe4-210">**Cím** – az értesítés informatív címet</span><span class="sxs-lookup"><span data-stu-id="51fe4-210">**Title** – the descriptive title of the notification</span></span>

    -   <span data-ttu-id="51fe4-211">Példa – **Application proxy beállításai**</span><span class="sxs-lookup"><span data-stu-id="51fe4-211">Example – **Application proxy settings**</span></span>

-   <span data-ttu-id="51fe4-212">**Leírás** – Mi történt a művelet leírása</span><span class="sxs-lookup"><span data-stu-id="51fe4-212">**Description** – the description of what occurred as a result of the operation</span></span>

    -   <span data-ttu-id="51fe4-213">Példa – **megadott belső URL-cím már használatban van egy másik alkalmazás.**</span><span class="sxs-lookup"><span data-stu-id="51fe4-213">Example – **Internal url entered is already being used by another application**</span></span>

-   <span data-ttu-id="51fe4-214">**Értesítés azonosítója** – az értesítés egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="51fe4-214">**Notification Id** – the unique id of the notification</span></span>

    -   <span data-ttu-id="51fe4-215">Példa – **clientNotification-2adbfc06-2073-4678-a69f-7eb78d96b068**</span><span class="sxs-lookup"><span data-stu-id="51fe4-215">Example – **clientNotification-2adbfc06-2073-4678-a69f-7eb78d96b068**</span></span>

-   <span data-ttu-id="51fe4-216">**Ügyfélkérelem-azonosító** – a böngésző által végrehajtott egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="51fe4-216">**Client Request Id** – the specific request id made by your browser</span></span>

    -   <span data-ttu-id="51fe4-217">Példa – **302fd775-3329-4670-a9f3-bea37004f0bc**</span><span class="sxs-lookup"><span data-stu-id="51fe4-217">Example – **302fd775-3329-4670-a9f3-bea37004f0bc**</span></span>

-   <span data-ttu-id="51fe4-218">**Stamp UTC-idő** – a timestamp, amely során az értesítés történt UTC szerint</span><span class="sxs-lookup"><span data-stu-id="51fe4-218">**Time Stamp UTC** – the timestamp during which the notification occurred, in UTC</span></span>

    -   <span data-ttu-id="51fe4-219">Példa – **2017-03-23T19:50:43.7583681Z**</span><span class="sxs-lookup"><span data-stu-id="51fe4-219">Example – **2017-03-23T19:50:43.7583681Z**</span></span>

-   <span data-ttu-id="51fe4-220">**Belső tranzakció azonosítója** – belső azonosítója a Microsoft segítségével, a rendszer keresse meg a hiba</span><span class="sxs-lookup"><span data-stu-id="51fe4-220">**Internal Transaction Id** – the internal ID we can use to look the error up in our systems</span></span>

    -   <span data-ttu-id="51fe4-221">Példa – **71a2f329-ca29-402f-aa72-bc00a7aca603**</span><span class="sxs-lookup"><span data-stu-id="51fe4-221">Example – **71a2f329-ca29-402f-aa72-bc00a7aca603**</span></span>

-   <span data-ttu-id="51fe4-222">**Egyszerű felhasználónév** – a műveletet végző felhasználó</span><span class="sxs-lookup"><span data-stu-id="51fe4-222">**UPN** – the user who performed the operation</span></span>

    -   <span data-ttu-id="51fe4-223">– Példa**tperkins@f128.info**</span><span class="sxs-lookup"><span data-stu-id="51fe4-223">Example – **tperkins@f128.info**</span></span>

-   <span data-ttu-id="51fe4-224">**A bérlői azonosító** – az egyedi azonosító a bérlő által, hogy a művelet a felhasználó tagja volt.</span><span class="sxs-lookup"><span data-stu-id="51fe4-224">**Tenant Id** – the unique ID of the tenant that the user who performed the operation was a member of</span></span>

    -   <span data-ttu-id="51fe4-225">Példa – **7918d4b5-0442-4a97-be2d-36f9f9962ece**</span><span class="sxs-lookup"><span data-stu-id="51fe4-225">Example – **7918d4b5-0442-4a97-be2d-36f9f9962ece**</span></span>

-   <span data-ttu-id="51fe4-226">**Felhasználói objektum azonosítója** – a művelet a felhasználó egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="51fe4-226">**User object Id** – the unique ID of the user who performed the operation</span></span>

    -   <span data-ttu-id="51fe4-227">Példa – **17f84be4-51f8-483a-b533-383791227a99**</span><span class="sxs-lookup"><span data-stu-id="51fe4-227">Example – **17f84be4-51f8-483a-b533-383791227a99**</span></span>

### <a name="detailed-notification-items"></a><span data-ttu-id="51fe4-228">Részletes értesítési elemek</span><span class="sxs-lookup"><span data-stu-id="51fe4-228">Detailed Notification Items</span></span>

-   <span data-ttu-id="51fe4-229">**Megjelenítendő név** – **(üres is lehet)** a hiba részletes megjelenítendő neve</span><span class="sxs-lookup"><span data-stu-id="51fe4-229">**Display Name** – **(can be empty)** a more detailed display name for the error</span></span>

    -   <span data-ttu-id="51fe4-230">Példa * – **Application proxy beállításai**</span><span class="sxs-lookup"><span data-stu-id="51fe4-230">Example *– **Application proxy settings**</span></span>

-   <span data-ttu-id="51fe4-231">**Állapot** – az értesítésre adott állapota</span><span class="sxs-lookup"><span data-stu-id="51fe4-231">**Status** – the specific status of the notification</span></span>

    -   <span data-ttu-id="51fe4-232">Példa * – **nem sikerült**</span><span class="sxs-lookup"><span data-stu-id="51fe4-232">Example *– **Failed**</span></span>

-   <span data-ttu-id="51fe4-233">**Objektumazonosító:** – **(üres is lehet)** az objektum azonosítója, amely alapján a művelet végrehajtása</span><span class="sxs-lookup"><span data-stu-id="51fe4-233">**Object Id** – **(can be empty)** the object ID against which the operation was performed</span></span>

    -   <span data-ttu-id="51fe4-234">Példa – **8e08161d-f2fd-40ad-a34a-a9632d6bb599**</span><span class="sxs-lookup"><span data-stu-id="51fe4-234">Example – **8e08161d-f2fd-40ad-a34a-a9632d6bb599**</span></span>

-   <span data-ttu-id="51fe4-235">**Részletek** – a részletes Mi történt a művelet leírása</span><span class="sxs-lookup"><span data-stu-id="51fe4-235">**Details** – the detailed description of what occurred as a result of the operation</span></span>

    -   <span data-ttu-id="51fe4-236">Példa – **belső URL-cím "http://bing.com/" érvénytelen, mert már használatban van**</span><span class="sxs-lookup"><span data-stu-id="51fe4-236">Example – **Internal url 'http://bing.com/' is invalid since it is already in use**</span></span>

-   <span data-ttu-id="51fe4-237">**Másolja át a hiba** – kattintson a **másolás ikon** jobb oldalán a **hiba másolása** szövegmező megosztani a támogatási szolgálathoz vagy a termék csoport mérnöke értesítés részleteinek másolása</span><span class="sxs-lookup"><span data-stu-id="51fe4-237">**Copy error** – Click the **copy icon** to the right of the **Copy error** textbox to copy all the notification details to share with a support or product group engineer</span></span>

    -   <span data-ttu-id="51fe4-238">– Példa```{"errorCode":"InternalUrl\_Duplicate","localizedErrorDetails":{"errorDetail":"Internal url 'http://google.com/' is invalid since it is already in use"},"operationResults":\[{"objectId":null,"displayName":null,"status":0,"details":"Internal url 'http://bing.com/' is invalid since it is already in use"}\],"timeStampUtc":"2017-03-23T19:50:26.465743Z","clientRequestId":"302fd775-3329-4670-a9f3-bea37004f0bb","internalTransactionId":"ea5b5475-03b9-4f08-8e95-bbb11289ab65","upn":"tperkins@f128.info","tenantId":"7918d4b5-0442-4a97-be2d-36f9f9962ece","userObjectId":"17f84be4-51f8-483a-b533-383791227a99"}```</span><span class="sxs-lookup"><span data-stu-id="51fe4-238">Example – ```{"errorCode":"InternalUrl\_Duplicate","localizedErrorDetails":{"errorDetail":"Internal url 'http://google.com/' is invalid since it is already in use"},"operationResults":\[{"objectId":null,"displayName":null,"status":0,"details":"Internal url 'http://bing.com/' is invalid since it is already in use"}\],"timeStampUtc":"2017-03-23T19:50:26.465743Z","clientRequestId":"302fd775-3329-4670-a9f3-bea37004f0bb","internalTransactionId":"ea5b5475-03b9-4f08-8e95-bbb11289ab65","upn":"tperkins@f128.info","tenantId":"7918d4b5-0442-4a97-be2d-36f9f9962ece","userObjectId":"17f84be4-51f8-483a-b533-383791227a99"}```</span></span>

## <a name="next-steps"></a><span data-ttu-id="51fe4-239">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="51fe4-239">Next steps</span></span>
[<span data-ttu-id="51fe4-240">Adja meg az egyszeri bejelentkezés az alkalmazásokba a Proxy</span><span class="sxs-lookup"><span data-stu-id="51fe4-240">Provide single sign-on to your apps with Application Proxy</span></span>](active-directory-application-proxy-sso-using-kcd.md)

