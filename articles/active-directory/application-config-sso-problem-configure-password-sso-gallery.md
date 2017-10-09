---
title: "jelszó egyszeri bejelentkezés az Azure AD-katalógusában alkalmazás konfigurálása aaaProblem |} Microsoft Docs"
description: "Hello gyakori problémák személyek arcfelismerési áttekinteni jelszó egyszeri bejelentkezéshez már szereplő alkalmazások számára az Azure AD Application Gallery hello konfigurálása"
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
ms.openlocfilehash: 78c37c52453c375bf7ccbca6df5c9008be4ce642
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="problem-configuring-password-single-sign-on-for-an-azure-ad-gallery-application"></a>A probléma jelszó egyszeri bejelentkezés az Azure AD-katalógusában alkalmazás konfigurálása

Ez a cikk segítséget toounderstand hello gyakori problémák személyek arcfelismerési konfigurálásakor **jelszó egyszeri bejelentkezés** Azure AD-katalógusában alkalmazással.

## <a name="credentials-are-filled-in-but-hello-extension-does-not-submit-them"></a>Kitölti a hitelesítő adatokat, de hello bővítmény elküldeni őket

Ez általában akkor fordul elő, ha hello alkalmazás gyártójának módosultak a bejelentkezési lapon nemrég tooadd mező, módosítsa az alapul szolgáló azonosítója használtuk toodetect hello felhasználónév és jelszó megadására, vagy módosítsa a hogyan hello bejelentkezés tapasztalhat az alkalmazásokban működik. Szerencsére sok esetben Microsoft együttműködhet alkalmazás szállítók toorapidly hárítsa el ezeket a problémákat.

Míg a Microsoft észlelés, ha ezek Integrációk törés kérjük, de egyes esetekben nem képes toofind technológiák tooautomatically ezeket a problémákat jobb funkcióval, illetve igénybe vehet néhány toofix eltöltött idő Hello esetben ezek Integrációk egyike sem működik megfelelően, ha szeretné Köszönjük támogatási esetet megnyitásakor, így a lehető leggyorsabban kérjük.

Továbbá toothis, a **szerepelnek a az alkalmazás gyártójával, ha** **küldje el a módon** , azt is dolgozhat velük toonatively alkalmazását integrálása az Azure Active Directoryban. Hello szállító toohello küldhet [listázása az alkalmazás hello Azure Active Directory alkalmazáskatalógusában](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing) tooget őket elindult.

## <a name="credentials-are-filled-in-and-submitted-but-hello-page-indicates-hello-credentials-are-incorrect"></a>Hitelesítő adatok kitöltötte és elküldve, de hello oldal azt jelzi, hogy hello hitelesítő adatok helytelenek

a probléma első ellenőrizze hello következő tooresolve:

-   Először próbálja túl hello felhasználó**közvetlenül bejelentkezési toohello alkalmazás webhelyét** számukra tárolt hello hitelesítő adatokkal.

  * Biztosan működik, ha már rendelkezik a hello kattintson hello felhasználói **hitelesítő adatainak frissítése** hello gombjára **alkalmazás csempe** a hello **alkalmazások** hello szakasza [alkalmazás Hozzáférési Panel](https://myapps.microsoft.com/) tooupdate őket toohello legújabb ismert felhasználónév és jelszó használata.

   * Ha meg, vagy egy másik hozzárendelt rendszergazda hello hitelesítő adatait a felhasználó találja hello felhasználóhoz vagy csoporthoz tartozó alkalmazás-hozzárendelés toohello navigálva **felhasználók és csoportok** hello alkalmazás hello hozzárendelés kiválasztása lapján és hello kattintva **frissítéséhez szükséges hitelesítő adatokat** gombra.

-   Hello felhasználó rendelve a saját hitelesítő adatait, ha rendelkezik hello felhasználói **tekintse meg arról, hogy a jelszavát nem lejárt hello alkalmazásban toobe** , és ha igen, **lejárt jelszófrissítési** toohello bejelentkezéssel az alkalmazás közvetlenül.

   * Hello jelszó hello alkalmazás frissítését, követően kérelem hello felhasználói tooclick hello **hitelesítő adatainak frissítése** hello gombjára **alkalmazás csempe** a hello **alkalmazások** hello szakasza [alkalmazás hozzáférési Panel](https://myapps.microsoft.com/) tooupdate őket toohello legújabb ismert felhasználónév és jelszó használata.

   * Ha meg, vagy egy másik hozzárendelt rendszergazda hello hitelesítő adatait a felhasználó találja hello felhasználóhoz vagy csoporthoz tartozó alkalmazás-hozzárendelés toohello navigálva **felhasználók és csoportok** hello alkalmazás hello hozzárendelés kiválasztása lapján és hello kattintva **frissítéséhez szükséges hitelesítő adatokat** gombra.

-   Hello felhasználó frissítés hello hozzáférési panel böngészőbővítmény rendelkezik hello hello az alábbi lépéseket követve [hogyan tooinstall hello hozzáférési Panel bővítmény](#how-to-install-the-access-panel-browser-extension) szakasz.

-   Győződjön meg arról, hogy a bővítmény hello hozzáférési panel fut, és a felhasználó böngészőben engedélyezve-e.

-   Győződjön meg arról, hogy a felhasználók nem próbálja hello hozzáférési panelén közben toohello alkalmazásban toosign **incognito, inPrivate vagy privát mód**. hello hozzáférési panel bővítmény e mód nem támogatott.

Abban az esetben, ha az nem működik, lehet, hogy olyan módosítás történt hello alkalmazás oldal, amely ideiglenesen megszeg hello alkalmazás integráció az Azure ad-val hello eset. Például okozhatja ezt hello alkalmazás gyártójának vezet be a lapon, amely manuális vs másképpen viselkedik parancsfájl automatikus bemeneti, aminek következtében automatikus integráció, például a saját, toobreak. Szerencsére sok esetben Microsoft együttműködhet alkalmazás szállítók toorapidly hárítsa el ezeket a problémákat.

Míg a Microsoft észlelés, ha ezek Integrációk törés kérjük, de egyes esetekben nem képes toofind technológiák tooautomatically ezeket a problémákat jobb funkcióval, illetve igénybe vehet néhány toofix eltöltött idő Hello esetben ezek Integrációk egyike sem működik megfelelően, ha szeretné Köszönjük támogatási esetet megnyitásakor, így a lehető leggyorsabban kérjük.

Továbbá toothis, a **szerepelnek a az alkalmazás gyártójával, ha** **küldje el a módon** , azt is dolgozhat velük toonatively alkalmazását integrálása az Azure Active Directoryban. Hello szállító toohello küldhet [listázása az alkalmazás hello Azure Active Directory alkalmazáskatalógusában](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing) tooget őket elindult.

## <a name="hello-extension-works-in-chrome-and-firefox-but-not-in-internet-explorer"></a>hello bővítmény működik a Chrome és a Firefox, de nem az Internet Explorerben

Ennek oka két fő toothis problémát:

-   Attól függően, hogy hello biztonsági beállítások engedélyezve van az Internet Explorerben, ha hello webhely nem része egy **megbízható zóna**, néha tiltható le a parancsfájl futtatásának hello alkalmazáshoz.

  *  tooresolve, ez túl utasítani hello felhasználói**hello alkalmazás webhely hozzáadása** toohello **megbízható helyek** listán belül a **Internet Explorer biztonsági beállításai**. A felhasználók toohello küldhet [hogyan tooadd egy hely toomy megbízható helyek listája](https://answers.microsoft.com/en-us/ie/forum/ie9-windows_7/how-do-i-add-a-site-to-my-trusted-sites-list/98cc77c8-b364-e011-8dfc-68b599b31bf5) cikk részletes utasításokat.

-   Ritka esetekben előfordulhat, hogy az Internet Explorer biztonsági ellenőrzés néha okozhat hello lap tooload a parancsfájl végrehajtásának hello lassabban.

   * Sajnos ez a helyzet eltérőek lehetnek attól függően, hello a böngésző verziója, a számítógép sebességétől vagy a meglátogatott webhely. Ebben az esetben javasoljuk, hogy kijavíthassuk hello integráció az adott alkalmazás támogatási szolgálatához.

Továbbá toothis, a **szerepelnek a az alkalmazás gyártójával, ha** **küldje el a módon** , azt is dolgozhat velük toonatively alkalmazását integrálása az Azure Active Directoryban. Hello szállító toohello küldhet [listázása az alkalmazás hello Azure Active Directory alkalmazáskatalógusában](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing) tooget őket elindult.

## <a name="check-if-hello-applications-login-page-has-changed-recently-or-requires-an-additional-field"></a>Ha hello alkalmazás bejelentkezési lapján mostanában megváltozott, vagy egy további mező szükséges ellenőrzése

Ha hello alkalmazás bejelentkezési oldalt a jelentősen megváltozott, néha ennek hatására a integrációja toobreak. Ez például akkor, ha az alkalmazás gyártójának hozzáadja egy bejelentkezési mezőben, a captcha, vagy a multi-factor authentication tootheir észlel. Szerencsére sok esetben Microsoft együttműködhet alkalmazás szállítók toorapidly hárítsa el ezeket a problémákat.

Míg a Microsoft észlelés, ha ezek Integrációk törés kérjük, de egyes esetekben nem képes toofind technológiák tooautomatically ezek problémákkal azonnal. Ellenkező esetben bizonyos idő toofix tartanak. Hello esetben ezek Integrációk egyike sem működik megfelelően, ha szeretné Köszönjük megnyitása támogatási esetet, így a lehető leggyorsabban kérjük.

Továbbá toothis, a **szerepelnek a az alkalmazás gyártójával, ha** **küldje el a módon** , azt is dolgozhat velük toonatively alkalmazását integrálása az Azure Active Directoryban. Hello szállító toohello küldhet [listázása az alkalmazás hello Azure Active Directory alkalmazáskatalógusában](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing) tooget őket elindult.

## <a name="how-tooinstall-hello-access-panel-browser-extension"></a>Hogyan tooinstall hello hozzáférési Panel bővítmény

tooinstall hello hozzáférési Panel bővítmény, kövesse az alábbi hello lépéseket:

1.  Nyissa meg hello [hozzáférési Panel](https://myapps.microsoft.com) valamelyik hello támogatott böngészők és való bejelentkezést egy **felhasználói** az Azure AD-ben.

2.  Kattintson egy **jelszó-SSO alkalmazás** a hozzáférési Panel hello.

3.  Hello Rákérdezés azzal a kérdéssel tooinstall hello szoftver, válassza ki **telepítés**.

4.  A böngésző alapján kell irányított toohello letöltési hivatkozását. **Adja hozzá** hello bővítmény tooyour böngésző.

5.  Ha a böngésző kéri, válassza ki a tooeither **engedélyezése** vagy **engedélyezése** hello bővítmény.

6.  Korábban telepítve, **indítsa újra a** a böngésző-munkamenetet.

7.  Jelentkezzen be a hozzáférési Panel hello, és tekintse meg, ha **indítása** a jelszó-egyszeri bejelentkezés alkalmazásokhoz

Az alábbi hello közvetlen hivatkozások Chrome és Firefox is letöltheti hello bővítmény:

-   [Chrome hozzáférési Panel bővítmény](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

-   [A Firefox hozzáférési Panel bővítmény](https://addons.mozilla.org/firefox/addon/access-panel-extension/)

## <a name="next-steps"></a>Következő lépések
[Adja meg az egyszeri bejelentkezés tooyour alkalmazások alkalmazásproxyval](active-directory-application-proxy-sso-using-kcd.md)

