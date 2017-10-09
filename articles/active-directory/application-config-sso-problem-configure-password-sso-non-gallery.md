---
title: "jelszó egyszeri bejelentkezés nem galéria alkalmazások konfigurálása aaaProblem |} Microsoft Docs"
description: "Hello gyakori problémák személyek arcfelismerési áttekinteni az Azure AD Application Gallery hello jelszó egyszeri bejelentkezést az egyéni, amelyek nem szerepelnek a listán nem galéria-alkalmazások konfigurálása"
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
ms.openlocfilehash: 3aee0a4c525bb3da338da2da0882ec572cf0e5e6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="problem-configuring-password-single-sign-on-for-a-non-gallery-application"></a><span data-ttu-id="04acd-103">A probléma jelszó egyszeri bejelentkezés nem galéria alkalmazások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="04acd-103">Problem configuring password single sign-on for a non-gallery application</span></span>

<span data-ttu-id="04acd-104">Ez a cikk segítséget toounderstand hello gyakori problémák személyek arcfelismerési konfigurálásakor **jelszó egyszeri bejelentkezés** nem galéria alkalmazással.</span><span class="sxs-lookup"><span data-stu-id="04acd-104">This article help you toounderstand hello common problems people face when configuring **Password single sign-on** with a non-gallery application.</span></span>

## <a name="how-toocapture-sign-in-fields-for-an-application"></a><span data-ttu-id="04acd-105">Hogyan toocapture bejelentkezés egy alkalmazás mezők</span><span class="sxs-lookup"><span data-stu-id="04acd-105">How toocapture sign-in fields for an application</span></span>

<span data-ttu-id="04acd-106">Bejelentkezési mező rögzítési csak a támogatott bejelentkezési lap HTML-kompatibilis, és **nem támogatott a nem szabványos bejelentkezési lapok**, például a Flash, vagy egyéb nem HTML-kompatibilis technológiákat használó.</span><span class="sxs-lookup"><span data-stu-id="04acd-106">Sign-in field capture is only supported for HTML-enabled sign-in pages and is **not supported for non-standard sign-in pages**, like those that use Flash, or other non-HTML-enabled technologies.</span></span>

<span data-ttu-id="04acd-107">Az egyéni alkalmazások rögzítheti a bejelentkezési mező két módja van:</span><span class="sxs-lookup"><span data-stu-id="04acd-107">There are two ways you can capture sign-in fields for your custom applications:</span></span>

-   <span data-ttu-id="04acd-108">Automatikus bejelentkezés mező rögzítése</span><span class="sxs-lookup"><span data-stu-id="04acd-108">Automatic sign-in field capture</span></span>

-   <span data-ttu-id="04acd-109">Manuális bejelentkezési mező rögzítése</span><span class="sxs-lookup"><span data-stu-id="04acd-109">Manual sign-in field capture</span></span>

<span data-ttu-id="04acd-110">**Automatikus bejelentkezés mező rögzítési** jól működik a legtöbb HTML-kompatibilis bejelentkezési lapok, ha azok **hello felhasználónevet és jelszót adjon meg a jól ismert DIV azonosítók** mező.</span><span class="sxs-lookup"><span data-stu-id="04acd-110">**Automatic sign-in field capture** works well with most HTML-enabled sign-in pages, if they use **well-known DIV IDs for hello username and password input** field.</span></span> <span data-ttu-id="04acd-111">hello a működés módja lekaparást hello HTML a hello lap toofind feltételeknek megfelelő DIV azonosítók és azt később visszajátszásos jelszavak tooit, majd mentse a, hogy az alkalmazás metaadatai.</span><span class="sxs-lookup"><span data-stu-id="04acd-111">hello way this works is by scraping hello HTML on hello page toofind DIV IDs that match certain criteria and by then saving that metadata for this application so we can replay passwords tooit later.</span></span>

<span data-ttu-id="04acd-112">**Manuális bejelentkezési mező rögzítési** alkalmazás hello hello esetekben használható **szállító nem címke** hello bemeneti bejelentkezéshez használt mezőkkel.</span><span class="sxs-lookup"><span data-stu-id="04acd-112">**Manual sign-in field capture** can be used in hello case that hello application **vendor does not label** hello input fields used for sign in.</span></span> <span data-ttu-id="04acd-113">Manuális bejelentkezési mező rögzítési is használható hello esetben, amikor hello **szállító Renderelés több mező** lehet, amely automatikusan észleli.</span><span class="sxs-lookup"><span data-stu-id="04acd-113">Manual sign-in field capture can also be used in hello case when hello **vendor renders multiple fields** which cannot be auto-detected.</span></span> <span data-ttu-id="04acd-114">Az Azure AD is tárolja az adatokat a sok mezőből áll, amelyek hello a Bejelentkezés lapon mindaddig, amíg Ön mondja el, ahol ezek a mezők vannak hello oldalon.</span><span class="sxs-lookup"><span data-stu-id="04acd-114">Azure AD can store data for as many fields as are on hello sign in page, so long as you tell us where those fields are on hello page.</span></span>

<span data-ttu-id="04acd-115">Általában **automatikus bejelentkezési mező rögzítése nem működik, ha mindig javasoljuk, hogy közben hello manuális beállítás.**</span><span class="sxs-lookup"><span data-stu-id="04acd-115">In general, **if automatic sign-in field capture does not work, we always suggest trying hello manual option.**</span></span>

### <a name="how-tooautomatically-capture-sign-in-fields-for-an-application"></a><span data-ttu-id="04acd-116">Hogyan tooautomatically rögzítése az alkalmazás bejelentkezési mezők</span><span class="sxs-lookup"><span data-stu-id="04acd-116">How tooautomatically capture sign-in fields for an application</span></span>

<span data-ttu-id="04acd-117">tooconfigure **jelszó-alapú egyszeri bejelentkezést** egy alkalmazást, amely a **automatikus bejelentkezési mező rögzítési**, kövesse az alábbi hello lépéseket:</span><span class="sxs-lookup"><span data-stu-id="04acd-117">tooconfigure **Password-based Single Sign-on** for an application using **automatic sign-in field capture**, follow hello steps below:</span></span>

1.  <span data-ttu-id="04acd-118">Nyitott hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**</span><span class="sxs-lookup"><span data-stu-id="04acd-118">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="04acd-119">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="04acd-119">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="04acd-120">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="04acd-120">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="04acd-121">Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="04acd-121">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="04acd-122">Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="04acd-122">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="04acd-123">Ha hello alkalmazás jelenik itt meg nem látja, akkor hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját** és set hello **megjelenítése** beállítás túl **Minden alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="04acd-123">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="04acd-124">Válassza ki a kívánt tooconfigure egyszeri bejelentkezés hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="04acd-124">Select hello application you want tooconfigure single sign-on.</span></span>

7.  <span data-ttu-id="04acd-125">Ha hello alkalmazás betölt, kattintson a hello **egyszeri bejelentkezés** hello alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="04acd-125">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="04acd-126">Jelölje be hello mód **jelszóalapú bejelentkezés.**</span><span class="sxs-lookup"><span data-stu-id="04acd-126">Select hello mode **Password-based Sign-on.**</span></span>

9.  <span data-ttu-id="04acd-127">Adja meg a hello **bejelentkezési URL-cím**.</span><span class="sxs-lookup"><span data-stu-id="04acd-127">Enter hello **Sign-on URL**.</span></span> <span data-ttu-id="04acd-128">Ez a hello URL-cím, ahol felhasználók adja meg a felhasználónevet és jelszót toosign a számára.</span><span class="sxs-lookup"><span data-stu-id="04acd-128">This is hello URL where users enter their username and password toosign in to.</span></span> <span data-ttu-id="04acd-129">**Biztosítsa, hogy hello bejelentkezési mezőben látható megadta hello URL-címen**.</span><span class="sxs-lookup"><span data-stu-id="04acd-129">**Ensure hello sign in fields are visible at hello URL you provide**.</span></span>

10. <span data-ttu-id="04acd-130">Kattintson a hello **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="04acd-130">Click hello **Save** button.</span></span>

11. <span data-ttu-id="04acd-131">Teheti meg, amely azt fogja automatikusan scrape URL-címet a felhasználónév és jelszó beviteli mező, és lehetővé teszik toouse az Azure AD toosecurely továbbítja a jelszavakat toothat alkalmazás hello hozzáférési panel bővítmény használatával.</span><span class="sxs-lookup"><span data-stu-id="04acd-131">Once you do that, we’ll automatically scrape that URL for a username and password input box and allow you toouse Azure AD toosecurely transmit passwords toothat application using hello access panel browser extension.</span></span>

## <a name="how-toomanually-capture-sign-in-fields-for-an-application"></a><span data-ttu-id="04acd-132">Hogyan toomanually rögzítése az alkalmazás bejelentkezési mezők</span><span class="sxs-lookup"><span data-stu-id="04acd-132">How toomanually capture sign-in fields for an application</span></span>

<span data-ttu-id="04acd-133">mezők toomanually rögzítési bejelentkezés kell hello hozzáférési Panel bővítmény telepítve és **nem fut az InPrivate-, incognito vagy privát módban.**</span><span class="sxs-lookup"><span data-stu-id="04acd-133">toomanually capture sign in fields, you must first have hello Access Panel Browser extension installed and **not be running in inPrivate, incognito, or private mode.**</span></span> <span data-ttu-id="04acd-134">tooinstall hello böngészőbővítmény, hajtsa végre hello lépéseit hello [hogyan tooinstall hello hozzáférési Panel bővítmény](#i-cannot-manually-detect-sign-in-fields-for-my-application) szakasz.</span><span class="sxs-lookup"><span data-stu-id="04acd-134">tooinstall hello browser extension, follow hello steps in hello [How tooinstall hello Access Panel Browser extension](#i-cannot-manually-detect-sign-in-fields-for-my-application) section.</span></span>

<span data-ttu-id="04acd-135">tooconfigure **jelszó-alapú egyszeri bejelentkezést** egy alkalmazást, amely a **manuális bejelentkezési mező rögzítési**, kövesse az alábbi hello lépéseket:</span><span class="sxs-lookup"><span data-stu-id="04acd-135">tooconfigure **Password-based Single Sign-on** for an application using **manual sign-in field capture**, follow hello steps below:</span></span>

1.  <span data-ttu-id="04acd-136">Nyitott hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**</span><span class="sxs-lookup"><span data-stu-id="04acd-136">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="04acd-137">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="04acd-137">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="04acd-138">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="04acd-138">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="04acd-139">Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="04acd-139">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="04acd-140">Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="04acd-140">click **All Applications** tooview a list of all your applications.</span></span>

   * <span data-ttu-id="04acd-141">Ha hello alkalmazás jelenik itt meg nem látja, akkor hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját** és set hello **megjelenítése** beállítás túl **Minden alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="04acd-141">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="04acd-142">Válassza ki a kívánt tooconfigure egyszeri bejelentkezés hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="04acd-142">Select hello application you want tooconfigure single sign-on.</span></span>

7.  <span data-ttu-id="04acd-143">Ha hello alkalmazás betölt, kattintson a hello **egyszeri bejelentkezés** hello alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="04acd-143">Once hello application loads, click hello **Single sign-on** from hello application’s left hand navigation menu.</span></span>

8.  <span data-ttu-id="04acd-144">Jelölje be hello mód **jelszóalapú bejelentkezés.**</span><span class="sxs-lookup"><span data-stu-id="04acd-144">Select hello mode **Password-based Sign-on.**</span></span>

9.  <span data-ttu-id="04acd-145">Adja meg a hello **bejelentkezési URL-cím**.</span><span class="sxs-lookup"><span data-stu-id="04acd-145">Enter hello **Sign-on URL**.</span></span> <span data-ttu-id="04acd-146">Ez a hello URL-cím, ahol felhasználók adja meg a felhasználónevet és jelszót toosign a számára.</span><span class="sxs-lookup"><span data-stu-id="04acd-146">This is hello URL where users enter their username and password toosign in to.</span></span> <span data-ttu-id="04acd-147">**Biztosítsa, hogy hello bejelentkezési mezőben látható megadta hello URL-címen**.</span><span class="sxs-lookup"><span data-stu-id="04acd-147">**Ensure hello sign in fields are visible at hello URL you provide**.</span></span>

10. <span data-ttu-id="04acd-148">Kattintson a hello **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="04acd-148">Click hello **Save** button.</span></span>

11. <span data-ttu-id="04acd-149">Teheti meg, amely azt fogja automatikusan scrape URL-címet a felhasználónév és jelszó beviteli mező, és lehetővé teszik toouse az Azure AD toosecurely továbbítja a jelszavakat toothat alkalmazás hello hozzáférési panel bővítmény használatával.</span><span class="sxs-lookup"><span data-stu-id="04acd-149">Once you do that, we’ll automatically scrape that URL for a username and password input box and allow you toouse Azure AD toosecurely transmit passwords toothat application using hello access panel browser extension.</span></span> <span data-ttu-id="04acd-150">Hello esetében, hogy ez nem sikerül, akkor **hello bejelentkezési mód toouse manuális bejelentkezési mező rögzítési módosítása** folytatással toostep 12.</span><span class="sxs-lookup"><span data-stu-id="04acd-150">In hello case that this fails, you can **change hello sign-in mode toouse manual sign-in field capture** by continuing toostep 12.</span></span>

12. <span data-ttu-id="04acd-151">Kattintson a **konfigurálása &lt;appname&gt; jelszó egyszeri bejelentkezési beállítások**.</span><span class="sxs-lookup"><span data-stu-id="04acd-151">click **Configure &lt;appname&gt; Password Single Sign-on Settings**.</span></span>

13. <span data-ttu-id="04acd-152">Jelölje be hello **manuálisan észleli a bejelentkezési mező** konfigurációs beállítást.</span><span class="sxs-lookup"><span data-stu-id="04acd-152">Select hello **Manually detect sign-in fields** configuration option.</span></span>

14. <span data-ttu-id="04acd-153">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="04acd-153">Click **Ok**.</span></span>

15. <span data-ttu-id="04acd-154">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="04acd-154">Click **Save**.</span></span>

16. <span data-ttu-id="04acd-155">Hello kövesse a képernyőn utasításokat toouse hello hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="04acd-155">Follow hello on screen instructions toouse hello Access Panel.</span></span>

## <a name="i-see-a-we-couldnt-find-any-sign-in-fields-at-that-url-error"></a><span data-ttu-id="04acd-156">A "Minden bejelentkezés mezőt, hogy az URL-címen nem található" hiba</span><span class="sxs-lookup"><span data-stu-id="04acd-156">I see a “We couldn’t find any sign-in fields at that URL” error</span></span>

<span data-ttu-id="04acd-157">Ezt a hibaüzenetet látja, ha a bejelentkezési mezők automatikus észlelése nem sikerült.</span><span class="sxs-lookup"><span data-stu-id="04acd-157">You see this error when automatic detection of sign-in fields fails.</span></span> <span data-ttu-id="04acd-158">a problémát, próbálja meg manuális bejelentkezési mező észlelését, a következő hello szükséges lépések hello tooresolve [hogyan toomanually rögzítése az alkalmazás bejelentkezési mezők](#how-to-manually-capture-sign-in-fields-for-an-application) szakasz.</span><span class="sxs-lookup"><span data-stu-id="04acd-158">tooresolve this issue, try manual sign-in field detection by following hello steps in hello [How toomanually capture sign-in fields for an application](#how-to-manually-capture-sign-in-fields-for-an-application) section.</span></span>

## <a name="i-see-an-unable-toosave-single-sign-on-configuration-error"></a><span data-ttu-id="04acd-159">"Nem toosave-konfiguráció egyszeri bejelentkezést." hibaüzenet</span><span class="sxs-lookup"><span data-stu-id="04acd-159">I see an “Unable toosave Single Sign-on configuration” error</span></span>

<span data-ttu-id="04acd-160">Ritka esetekben hello egyszeri bejelentkezés konfigurációjának frissítése sikertelen lehet.</span><span class="sxs-lookup"><span data-stu-id="04acd-160">In certain rare cases, updating hello single sign-on configuration can fail.</span></span> <span data-ttu-id="04acd-161">a hello egyszeri bejelentkezés konfigurációjának mentése újra próbálja tooresolve.</span><span class="sxs-lookup"><span data-stu-id="04acd-161">tooresolve this try saving hello single sign-on configuration again.</span></span>

<span data-ttu-id="04acd-162">Ha ez továbbra is toofail következetesen, nyissa meg a támogatási esetet, és adja meg a hello összegyűjtött hello információt [hogyan toosee hello adatait a portál értesítései](#i-cannot-manually-detect-sign-in-fields-for-my-application) és [hogyan segíthet az tooget részletek tooa értesítés küldése támogatja a visszafejtés](#how-to-get-help-by-sending-notification-details-to-a-support-engineer) szakaszok.</span><span class="sxs-lookup"><span data-stu-id="04acd-162">If this continues toofail consistently, open a support case and provide hello information gathered in hello [How toosee hello details of a portal notification](#i-cannot-manually-detect-sign-in-fields-for-my-application) and [How tooget help by sending notification details tooa support engineer](#how-to-get-help-by-sending-notification-details-to-a-support-engineer) sections.</span></span>

## <a name="i-cannot-manually-detect-sign-in-fields-for-my-application"></a><span data-ttu-id="04acd-163">I nem manuálisan észlel bejelentkezési mezőket az alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="04acd-163">I cannot manually detect sign in fields for my application</span></span>

<span data-ttu-id="04acd-164">Előfordulhat, ha manuális észlelése nem működik látható hello viselkedésmódok közé:</span><span class="sxs-lookup"><span data-stu-id="04acd-164">Some of hello behaviors you might see when manual detection is not working include:</span></span>

-   <span data-ttu-id="04acd-165">hello manuális rögzítési folyamat toowork jelent meg, de nem volt megfelelő rögzített hello mezők</span><span class="sxs-lookup"><span data-stu-id="04acd-165">hello manual capture process appeared toowork, but hello fields captured were not correct</span></span>

-   <span data-ttu-id="04acd-166">hello jobb mezők nem beolvasni a kijelölt hello rögzítési folyamat végrehajtása során</span><span class="sxs-lookup"><span data-stu-id="04acd-166">hello right fields don’t get highlighted when performing hello capture process</span></span>

-   <span data-ttu-id="04acd-167">hello rögzítési folyamat veszi át me toohello alkalmazás bejelentkezési lap elvárásoknak megfelelően, de semmi nem történik</span><span class="sxs-lookup"><span data-stu-id="04acd-167">hello capture process takes me toohello application’s login page as expected, but nothing happens</span></span>

-   <span data-ttu-id="04acd-168">Kézi rögzítési toowork megjelenik, de SSO nem kerül sor, amikor a felhasználók megnyitják a hozzáférési Panel hello toohello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="04acd-168">Manual capture appears toowork, but SSO doesn’t happen when my users navigate toohello application from hello Access Panel.</span></span>

<span data-ttu-id="04acd-169">Ha ezeket a problémákat tapasztal, ellenőrizze a hello következőket:</span><span class="sxs-lookup"><span data-stu-id="04acd-169">check hello following if you encounter any of these issues:</span></span>

-   <span data-ttu-id="04acd-170">Győződjön meg arról, hogy rendelkezik-e hello hozzáférési panel bővítmény legújabb verziója hello **telepített** és **engedélyezett** hello hello utasításait követve [hogyan tooinstall hello hozzáférési Panel böngésző bővítmény](#how-to-install-the-access-panel-browser-extension) szakasz.</span><span class="sxs-lookup"><span data-stu-id="04acd-170">Ensure that you have hello latest version of hello access panel browser extension **installed** and **enabled** by following hello steps in hello [How tooinstall hello Access Panel Browser extension](#how-to-install-the-access-panel-browser-extension) section.</span></span>

-   <span data-ttu-id="04acd-171">Győződjön meg arról, hogy nem kívánt hello rögzítési folyamat közben a böngésző a **incognito, inPrivate vagy privát mód**.</span><span class="sxs-lookup"><span data-stu-id="04acd-171">Ensure that you are not attempting hello capture process while your browser in **incognito, inPrivate, or Private mode**.</span></span> <span data-ttu-id="04acd-172">hello hozzáférési panel bővítmény e mód nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="04acd-172">hello access panel extension is not supported in these modes.</span></span>

-   <span data-ttu-id="04acd-173">Győződjön meg arról, hogy a felhasználók nem próbálja hello hozzáférési panelén közben toohello alkalmazásban toosign **incognito, inPrivate vagy privát mód**.</span><span class="sxs-lookup"><span data-stu-id="04acd-173">Ensure that your users are not trying toosign in toohello application from hello access panel while in **incognito, inPrivate, or Private mode**.</span></span> <span data-ttu-id="04acd-174">hello hozzáférési panel bővítmény e mód nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="04acd-174">hello access panel extension is not supported in these modes.</span></span>

-   <span data-ttu-id="04acd-175">Próbálja meg újból hello manuális rögzítését, piros hello jelölők teljesülnek keresztül hello megfelelő mezőket.</span><span class="sxs-lookup"><span data-stu-id="04acd-175">Try hello manual capture process again, ensuring hello red markers are over hello correct fields.</span></span>

-   <span data-ttu-id="04acd-176">Ha hello manuális rögzítési folyamat toohang úgy tűnik, vagy hello bejelentkezési oldal nem semmit (3. eset fent), próbálja hello manuális rögzítési folyamat újra.</span><span class="sxs-lookup"><span data-stu-id="04acd-176">If hello manual capture process seems toohang, or hello sign in page doesn’t do anything (case 3 above), try hello manual capture process again.</span></span> <span data-ttu-id="04acd-177">De hello folyamat, nyomja meg az hello befejezése után most **F12** tooopen gombra a böngésző fejlesztői konzolján.</span><span class="sxs-lookup"><span data-stu-id="04acd-177">But, this time after completing hello process, press hello **F12** button tooopen your browser’s developer console.</span></span> <span data-ttu-id="04acd-178">Egyszer, nyissa meg a hello **konzol** és típus **window.location= "&lt;hello bejelentkezési hello app konfigurálásakor megadott URL-címet adjon meg&gt;"** , és nyomja le az  **Adja meg**.</span><span class="sxs-lookup"><span data-stu-id="04acd-178">Once there, open hello **console** and type **window.location=”&lt;enter hello sign in url you specified when configuring hello app&gt;”** and then press **Enter**.</span></span> <span data-ttu-id="04acd-179">A kényszerített egy lap átirányítási hello rögzítési folyamat befejezéséhez és hello mezők rögzített kell tárolni.</span><span class="sxs-lookup"><span data-stu-id="04acd-179">This force a page redirect which end hello capture process and store hello fields that have been captured.</span></span>

<span data-ttu-id="04acd-180">Ha ezek egyike sem működik, meg, segítünk.</span><span class="sxs-lookup"><span data-stu-id="04acd-180">If none of these approaches work for you, we can help.</span></span> <span data-ttu-id="04acd-181">Nyisson meg egy támogatási esetet mi próbált, valamint hello összegyűjtött hello információt hello részleteit [hogyan toosee hello adatait a portál értesítései](#i-cannot-manually-detect-sign-in-fields-for-my-application) és [hogyan segíthet az tooget értesítési adatokat küldjenek tooa támogatása a visszafejtés](#how-to-get-help-by-sending-notification-details-to-a-support-engineer) részek (ha van ilyen).</span><span class="sxs-lookup"><span data-stu-id="04acd-181">Open a support case with hello details of what you tried, as well as hello information gathered in hello [How toosee hello details of a portal notification](#i-cannot-manually-detect-sign-in-fields-for-my-application) and [How tooget help by sending notification details tooa support engineer](#how-to-get-help-by-sending-notification-details-to-a-support-engineer) sections (if applicable).</span></span>

## <a name="how-tooinstall-hello-access-panel-browser-extension"></a><span data-ttu-id="04acd-182">Hogyan tooinstall hello hozzáférési Panel bővítmény</span><span class="sxs-lookup"><span data-stu-id="04acd-182">How tooinstall hello Access Panel Browser extension</span></span>

<span data-ttu-id="04acd-183">tooinstall hello hozzáférési Panel bővítmény, kövesse az alábbi hello lépéseket:</span><span class="sxs-lookup"><span data-stu-id="04acd-183">tooinstall hello Access Panel Browser extension, follow hello steps below:</span></span>

1.  <span data-ttu-id="04acd-184">Nyissa meg hello [hozzáférési Panel](https://myapps.microsoft.com) valamelyik hello támogatott böngészők és való bejelentkezést egy **felhasználói** az Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="04acd-184">Open hello [Access Panel](https://myapps.microsoft.com) in one of hello supported browsers and sign in as a **user** in your Azure AD.</span></span>

2.  <span data-ttu-id="04acd-185">Kattintson egy **jelszó-SSO alkalmazás** a hozzáférési Panel hello.</span><span class="sxs-lookup"><span data-stu-id="04acd-185">click a **password-SSO application** in hello Access Panel.</span></span>

3.  <span data-ttu-id="04acd-186">Hello Rákérdezés azzal a kérdéssel tooinstall hello szoftver, válassza ki **telepítés**.</span><span class="sxs-lookup"><span data-stu-id="04acd-186">In hello prompt asking tooinstall hello software, select **Install Now**.</span></span>

4.  <span data-ttu-id="04acd-187">A böngésző alapján kell irányított toohello letöltési hivatkozását.</span><span class="sxs-lookup"><span data-stu-id="04acd-187">Based on your browser you be directed toohello download link.</span></span> <span data-ttu-id="04acd-188">**Adja hozzá** hello bővítmény tooyour böngésző.</span><span class="sxs-lookup"><span data-stu-id="04acd-188">**Add** hello extension tooyour browser.</span></span>

5.  <span data-ttu-id="04acd-189">Ha a böngésző kéri, válassza ki a tooeither **engedélyezése** vagy **engedélyezése** hello bővítmény.</span><span class="sxs-lookup"><span data-stu-id="04acd-189">If your browser asks, select tooeither **Enable** or **Allow** hello extension.</span></span>

6.  <span data-ttu-id="04acd-190">Korábban telepítve, **indítsa újra a** a böngésző-munkamenetet.</span><span class="sxs-lookup"><span data-stu-id="04acd-190">Once installed, **restart** your browser session.</span></span>

7.  <span data-ttu-id="04acd-191">Jelentkezzen be a hozzáférési Panel hello, és tekintse meg, ha **indítása** a jelszó-egyszeri bejelentkezés alkalmazásokhoz.</span><span class="sxs-lookup"><span data-stu-id="04acd-191">Sign in into hello Access Panel and see if you can **launch** your password-SSO applications.</span></span>

<span data-ttu-id="04acd-192">Az alábbi hello közvetlen hivatkozások Chrome és Firefox is letöltheti hello bővítmény:</span><span class="sxs-lookup"><span data-stu-id="04acd-192">You may also download hello extension for Chrome and Firefox from hello direct links below:</span></span>

-   [<span data-ttu-id="04acd-193">Chrome hozzáférési Panel bővítmény</span><span class="sxs-lookup"><span data-stu-id="04acd-193">Chrome Access Panel Extension</span></span>](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

-   [<span data-ttu-id="04acd-194">A Firefox hozzáférési Panel bővítmény</span><span class="sxs-lookup"><span data-stu-id="04acd-194">Firefox Access Panel Extension</span></span>](https://addons.mozilla.org/firefox/addon/access-panel-extension/)

## <a name="how-toosee-hello-details-of-a-portal-notification"></a><span data-ttu-id="04acd-195">Hogyan toosee hello adatait a portál értesítései</span><span class="sxs-lookup"><span data-stu-id="04acd-195">How toosee hello details of a portal notification</span></span>

<span data-ttu-id="04acd-196">A portál értesítései hello részleteit láthatja hello alábbi lépéseket követve:</span><span class="sxs-lookup"><span data-stu-id="04acd-196">You can see hello details of any portal notification by following hello steps below:</span></span>

1.  <span data-ttu-id="04acd-197">Kattintson a hello **értesítések** hello hello Azure portál jobb felső sarkában (hello harang) ikonra</span><span class="sxs-lookup"><span data-stu-id="04acd-197">click hello **Notifications** icon (hello bell) in hello upper right of hello Azure Portal</span></span>

2.  <span data-ttu-id="04acd-198">Válassza ki az értesítésekhez egy **hiba** (piros (!) következő toothem rendelkezők) állapotát.</span><span class="sxs-lookup"><span data-stu-id="04acd-198">Select any notification in an **Error** state (those with a red (!) next toothem).</span></span>

  ><span data-ttu-id="04acd-199">! Vegye figyelembe], nem kattintson az értesítések egy **sikeres** vagy **folyamatban lévő** állapotát.</span><span class="sxs-lookup"><span data-stu-id="04acd-199">!NOTE] You cannot click notifications in a **Successful** or **In Progress** state.</span></span>
  >
  >

3.  <span data-ttu-id="04acd-200">A nyitott hello **értesítési részletek** panelen.</span><span class="sxs-lookup"><span data-stu-id="04acd-200">This open hello **Notification Details** blade.</span></span>

4.  <span data-ttu-id="04acd-201">Ezt az információt használja saját kezűleg toounderstand több hello probléma vonatkozó részletek.</span><span class="sxs-lookup"><span data-stu-id="04acd-201">Use this information yourself toounderstand more details about hello problem.</span></span>

5.  <span data-ttu-id="04acd-202">Ha további segítségre van, is megoszthatja ezeket az információkat a támogatási szakértőhöz vagy hello csoport tooget Terméksúgó a probléma megoldásában.</span><span class="sxs-lookup"><span data-stu-id="04acd-202">If you still need help, you can also share this information with a support engineer or hello product group tooget help with your problem.</span></span>

6.  <span data-ttu-id="04acd-203">Hello kattintson **másolási** **ikon** sarkában hello toohello **másolja át a hiba** szövegmező toocopy összes hello értesítési részletek tooshare és a támogatási szolgálathoz vagy a termék csoport mérnöke.</span><span class="sxs-lookup"><span data-stu-id="04acd-203">Click hello **copy** **icon** toohello right of hello **Copy error** textbox toocopy all hello notification details tooshare with a support or product group engineer.</span></span>

## <a name="how-tooget-help-by-sending-notification-details-tooa-support-engineer"></a><span data-ttu-id="04acd-204">Hogyan segíthet az tooget értesítési részletek tooa támogatási szakértőhöz küldése</span><span class="sxs-lookup"><span data-stu-id="04acd-204">How tooget help by sending notification details tooa support engineer</span></span>

<span data-ttu-id="04acd-205">Nagyon fontos, hogy megosztott **összes alább felsorolt részletek hello** , ha segítségre van szüksége, így gyorsan segít a támogatási szakértőhöz.</span><span class="sxs-lookup"><span data-stu-id="04acd-205">It is very important that you share **all hello details listed below** with a support engineer if you need help, so that they can help you quickly.</span></span> <span data-ttu-id="04acd-206">Ehhez egyszerűen az **elkészíti a képernyőfelvételt** vagy hello kattintva **másolási hibaikon**, hello toohello jobbra található **hiba másolása** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="04acd-206">You can do this easily by **taking a screenshot,** or by clicking hello **Copy error icon**, found toohello right of hello **Copy error** textbox.</span></span>

## <a name="notification-details-explained"></a><span data-ttu-id="04acd-207">Értesítési részletek alapján</span><span class="sxs-lookup"><span data-stu-id="04acd-207">Notification Details Explained</span></span>

<span data-ttu-id="04acd-208">az alábbi hello több milyen egyes hello értesítési elemek jelent, és azok által biztosított példák ismerteti.</span><span class="sxs-lookup"><span data-stu-id="04acd-208">hello below explains more what each of hello notification items means, and gives examples of each of them.</span></span>

### <a name="essential-notification-items"></a><span data-ttu-id="04acd-209">Alapvető értesítési elemek</span><span class="sxs-lookup"><span data-stu-id="04acd-209">Essential Notification Items</span></span>

-   <span data-ttu-id="04acd-210">**Cím** – hello informatív címet hello értesítés</span><span class="sxs-lookup"><span data-stu-id="04acd-210">**Title** – hello descriptive title of hello notification</span></span>

    -   <span data-ttu-id="04acd-211">Példa – **Application proxy beállításai**</span><span class="sxs-lookup"><span data-stu-id="04acd-211">Example – **Application proxy settings**</span></span>

-   <span data-ttu-id="04acd-212">**Leírás** – hello Mi történt hello művelet leírása</span><span class="sxs-lookup"><span data-stu-id="04acd-212">**Description** – hello description of what occurred as a result of hello operation</span></span>

    -   <span data-ttu-id="04acd-213">Példa – **megadott belső URL-cím már használatban van egy másik alkalmazás.**</span><span class="sxs-lookup"><span data-stu-id="04acd-213">Example – **Internal url entered is already being used by another application**</span></span>

-   <span data-ttu-id="04acd-214">**Értesítés azonosítója** – hello értesítési hello egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="04acd-214">**Notification Id** – hello unique id of hello notification</span></span>

    -   <span data-ttu-id="04acd-215">Példa – **clientNotification-2adbfc06-2073-4678-a69f-7eb78d96b068**</span><span class="sxs-lookup"><span data-stu-id="04acd-215">Example – **clientNotification-2adbfc06-2073-4678-a69f-7eb78d96b068**</span></span>

-   <span data-ttu-id="04acd-216">**Ügyfélkérelem-azonosító** – a böngésző által végrehajtott hello adott kérelem azonosítója</span><span class="sxs-lookup"><span data-stu-id="04acd-216">**Client Request Id** – hello specific request id made by your browser</span></span>

    -   <span data-ttu-id="04acd-217">Példa – **302fd775-3329-4670-a9f3-bea37004f0bc**</span><span class="sxs-lookup"><span data-stu-id="04acd-217">Example – **302fd775-3329-4670-a9f3-bea37004f0bc**</span></span>

-   <span data-ttu-id="04acd-218">**Stamp UTC-idő** – hello időbélyeg időintervalluma hello értesítési történt UTC szerint</span><span class="sxs-lookup"><span data-stu-id="04acd-218">**Time Stamp UTC** – hello timestamp during which hello notification occurred, in UTC</span></span>

    -   <span data-ttu-id="04acd-219">Példa – **2017-03-23T19:50:43.7583681Z**</span><span class="sxs-lookup"><span data-stu-id="04acd-219">Example – **2017-03-23T19:50:43.7583681Z**</span></span>

-   <span data-ttu-id="04acd-220">**Belső tranzakció azonosítója** – hello használhatjuk toolook hello hiba a rendszer belső azonosítója</span><span class="sxs-lookup"><span data-stu-id="04acd-220">**Internal Transaction Id** – hello internal ID we can use toolook hello error up in our systems</span></span>

    -   <span data-ttu-id="04acd-221">Példa – **71a2f329-ca29-402f-aa72-bc00a7aca603**</span><span class="sxs-lookup"><span data-stu-id="04acd-221">Example – **71a2f329-ca29-402f-aa72-bc00a7aca603**</span></span>

-   <span data-ttu-id="04acd-222">**Egyszerű felhasználónév** – hello műveletet végre hello felhasználó</span><span class="sxs-lookup"><span data-stu-id="04acd-222">**UPN** – hello user who performed hello operation</span></span>

    -   <span data-ttu-id="04acd-223">– Példa**tperkins@f128.info**</span><span class="sxs-lookup"><span data-stu-id="04acd-223">Example – **tperkins@f128.info**</span></span>

-   <span data-ttu-id="04acd-224">**A bérlői azonosító** – hello egyedi Azonosítóját hello olyan bérlő számára, felhasználói hello hello műveletet végrehajtó tagja volt.</span><span class="sxs-lookup"><span data-stu-id="04acd-224">**Tenant Id** – hello unique ID of hello tenant that hello user who performed hello operation was a member of</span></span>

    -   <span data-ttu-id="04acd-225">Példa – **7918d4b5-0442-4a97-be2d-36f9f9962ece**</span><span class="sxs-lookup"><span data-stu-id="04acd-225">Example – **7918d4b5-0442-4a97-be2d-36f9f9962ece**</span></span>

-   <span data-ttu-id="04acd-226">**Felhasználói objektum azonosítója** – hello hello műveletet végre hello felhasználó egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="04acd-226">**User object Id** – hello unique ID of hello user who performed hello operation</span></span>

    -   <span data-ttu-id="04acd-227">Példa – **17f84be4-51f8-483a-b533-383791227a99**</span><span class="sxs-lookup"><span data-stu-id="04acd-227">Example – **17f84be4-51f8-483a-b533-383791227a99**</span></span>

### <a name="detailed-notification-items"></a><span data-ttu-id="04acd-228">Részletes értesítési elemek</span><span class="sxs-lookup"><span data-stu-id="04acd-228">Detailed Notification Items</span></span>

-   <span data-ttu-id="04acd-229">**Megjelenítendő név** – **(üres is lehet)** hello hiba részletes megjelenítendő neve</span><span class="sxs-lookup"><span data-stu-id="04acd-229">**Display Name** – **(can be empty)** a more detailed display name for hello error</span></span>

    -   <span data-ttu-id="04acd-230">Példa * – **Application proxy beállításai**</span><span class="sxs-lookup"><span data-stu-id="04acd-230">Example *– **Application proxy settings**</span></span>

-   <span data-ttu-id="04acd-231">**Állapot** – hello hello értesítési adott állapota</span><span class="sxs-lookup"><span data-stu-id="04acd-231">**Status** – hello specific status of hello notification</span></span>

    -   <span data-ttu-id="04acd-232">Példa * – **nem sikerült**</span><span class="sxs-lookup"><span data-stu-id="04acd-232">Example *– **Failed**</span></span>

-   <span data-ttu-id="04acd-233">**Objektumazonosító:** – **(üres is lehet)** hello Objektumazonosító alapján mely hello művelet végrehajtása</span><span class="sxs-lookup"><span data-stu-id="04acd-233">**Object Id** – **(can be empty)** hello object ID against which hello operation was performed</span></span>

    -   <span data-ttu-id="04acd-234">Példa – **8e08161d-f2fd-40ad-a34a-a9632d6bb599**</span><span class="sxs-lookup"><span data-stu-id="04acd-234">Example – **8e08161d-f2fd-40ad-a34a-a9632d6bb599**</span></span>

-   <span data-ttu-id="04acd-235">**Részletek** – hello részletes Mi történt hello művelet leírása</span><span class="sxs-lookup"><span data-stu-id="04acd-235">**Details** – hello detailed description of what occurred as a result of hello operation</span></span>

    -   <span data-ttu-id="04acd-236">Példa – **belső URL-cím "http://bing.com/" érvénytelen, mert már használatban van**</span><span class="sxs-lookup"><span data-stu-id="04acd-236">Example – **Internal url 'http://bing.com/' is invalid since it is already in use**</span></span>

-   <span data-ttu-id="04acd-237">**Másolja át a hiba** – hello kattintson **másolás ikon** sarkában hello toohello **hiba másolása** szövegmező toocopy összes hello értesítési részletek tooshare a támogatási szolgálathoz vagy a termék csoport visszafejtés</span><span class="sxs-lookup"><span data-stu-id="04acd-237">**Copy error** – Click hello **copy icon** toohello right of hello **Copy error** textbox toocopy all hello notification details tooshare with a support or product group engineer</span></span>

    -   <span data-ttu-id="04acd-238">– Példa```{"errorCode":"InternalUrl\_Duplicate","localizedErrorDetails":{"errorDetail":"Internal url 'http://google.com/' is invalid since it is already in use"},"operationResults":\[{"objectId":null,"displayName":null,"status":0,"details":"Internal url 'http://bing.com/' is invalid since it is already in use"}\],"timeStampUtc":"2017-03-23T19:50:26.465743Z","clientRequestId":"302fd775-3329-4670-a9f3-bea37004f0bb","internalTransactionId":"ea5b5475-03b9-4f08-8e95-bbb11289ab65","upn":"tperkins@f128.info","tenantId":"7918d4b5-0442-4a97-be2d-36f9f9962ece","userObjectId":"17f84be4-51f8-483a-b533-383791227a99"}```</span><span class="sxs-lookup"><span data-stu-id="04acd-238">Example – ```{"errorCode":"InternalUrl\_Duplicate","localizedErrorDetails":{"errorDetail":"Internal url 'http://google.com/' is invalid since it is already in use"},"operationResults":\[{"objectId":null,"displayName":null,"status":0,"details":"Internal url 'http://bing.com/' is invalid since it is already in use"}\],"timeStampUtc":"2017-03-23T19:50:26.465743Z","clientRequestId":"302fd775-3329-4670-a9f3-bea37004f0bb","internalTransactionId":"ea5b5475-03b9-4f08-8e95-bbb11289ab65","upn":"tperkins@f128.info","tenantId":"7918d4b5-0442-4a97-be2d-36f9f9962ece","userObjectId":"17f84be4-51f8-483a-b533-383791227a99"}```</span></span>

## <a name="next-steps"></a><span data-ttu-id="04acd-239">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="04acd-239">Next steps</span></span>
[<span data-ttu-id="04acd-240">Adja meg az egyszeri bejelentkezés tooyour alkalmazások alkalmazásproxyval</span><span class="sxs-lookup"><span data-stu-id="04acd-240">Provide single sign-on tooyour apps with Application Proxy</span></span>](active-directory-application-proxy-sso-using-kcd.md)

