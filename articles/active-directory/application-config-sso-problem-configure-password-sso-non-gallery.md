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
# <a name="problem-configuring-password-single-sign-on-for-a-non-gallery-application"></a>A probléma jelszó egyszeri bejelentkezés nem galéria alkalmazások konfigurálása

Ez a cikk segítséget toounderstand hello gyakori problémák személyek arcfelismerési konfigurálásakor **jelszó egyszeri bejelentkezés** nem galéria alkalmazással.

## <a name="how-toocapture-sign-in-fields-for-an-application"></a>Hogyan toocapture bejelentkezés egy alkalmazás mezők

Bejelentkezési mező rögzítési csak a támogatott bejelentkezési lap HTML-kompatibilis, és **nem támogatott a nem szabványos bejelentkezési lapok**, például a Flash, vagy egyéb nem HTML-kompatibilis technológiákat használó.

Az egyéni alkalmazások rögzítheti a bejelentkezési mező két módja van:

-   Automatikus bejelentkezés mező rögzítése

-   Manuális bejelentkezési mező rögzítése

**Automatikus bejelentkezés mező rögzítési** jól működik a legtöbb HTML-kompatibilis bejelentkezési lapok, ha azok **hello felhasználónevet és jelszót adjon meg a jól ismert DIV azonosítók** mező. hello a működés módja lekaparást hello HTML a hello lap toofind feltételeknek megfelelő DIV azonosítók és azt később visszajátszásos jelszavak tooit, majd mentse a, hogy az alkalmazás metaadatai.

**Manuális bejelentkezési mező rögzítési** alkalmazás hello hello esetekben használható **szállító nem címke** hello bemeneti bejelentkezéshez használt mezőkkel. Manuális bejelentkezési mező rögzítési is használható hello esetben, amikor hello **szállító Renderelés több mező** lehet, amely automatikusan észleli. Az Azure AD is tárolja az adatokat a sok mezőből áll, amelyek hello a Bejelentkezés lapon mindaddig, amíg Ön mondja el, ahol ezek a mezők vannak hello oldalon.

Általában **automatikus bejelentkezési mező rögzítése nem működik, ha mindig javasoljuk, hogy közben hello manuális beállítás.**

### <a name="how-tooautomatically-capture-sign-in-fields-for-an-application"></a>Hogyan tooautomatically rögzítése az alkalmazás bejelentkezési mezők

tooconfigure **jelszó-alapú egyszeri bejelentkezést** egy alkalmazást, amely a **automatikus bejelentkezési mező rögzítési**, kövesse az alábbi hello lépéseket:

1.  Nyitott hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**

2.  Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.

3.  Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.

4.  Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.

5.  Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.

  * Ha hello alkalmazás jelenik itt meg nem látja, akkor hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját** és set hello **megjelenítése** beállítás túl **Minden alkalmazást.**

6.  Válassza ki a kívánt tooconfigure egyszeri bejelentkezés hello alkalmazást.

7.  Ha hello alkalmazás betölt, kattintson a hello **egyszeri bejelentkezés** hello alkalmazás bal oldali navigációs menüjében.

8.  Jelölje be hello mód **jelszóalapú bejelentkezés.**

9.  Adja meg a hello **bejelentkezési URL-cím**. Ez a hello URL-cím, ahol felhasználók adja meg a felhasználónevet és jelszót toosign a számára. **Biztosítsa, hogy hello bejelentkezési mezőben látható megadta hello URL-címen**.

10. Kattintson a hello **mentése** gombra.

11. Teheti meg, amely azt fogja automatikusan scrape URL-címet a felhasználónév és jelszó beviteli mező, és lehetővé teszik toouse az Azure AD toosecurely továbbítja a jelszavakat toothat alkalmazás hello hozzáférési panel bővítmény használatával.

## <a name="how-toomanually-capture-sign-in-fields-for-an-application"></a>Hogyan toomanually rögzítése az alkalmazás bejelentkezési mezők

mezők toomanually rögzítési bejelentkezés kell hello hozzáférési Panel bővítmény telepítve és **nem fut az InPrivate-, incognito vagy privát módban.** tooinstall hello böngészőbővítmény, hajtsa végre hello lépéseit hello [hogyan tooinstall hello hozzáférési Panel bővítmény](#i-cannot-manually-detect-sign-in-fields-for-my-application) szakasz.

tooconfigure **jelszó-alapú egyszeri bejelentkezést** egy alkalmazást, amely a **manuális bejelentkezési mező rögzítési**, kövesse az alábbi hello lépéseket:

1.  Nyitott hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**

2.  Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.

3.  Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.

4.  Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.

5.  Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.

   * Ha hello alkalmazás jelenik itt meg nem látja, akkor hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját** és set hello **megjelenítése** beállítás túl **Minden alkalmazást.**

6.  Válassza ki a kívánt tooconfigure egyszeri bejelentkezés hello alkalmazást.

7.  Ha hello alkalmazás betölt, kattintson a hello **egyszeri bejelentkezés** hello alkalmazás bal oldali navigációs menüjében.

8.  Jelölje be hello mód **jelszóalapú bejelentkezés.**

9.  Adja meg a hello **bejelentkezési URL-cím**. Ez a hello URL-cím, ahol felhasználók adja meg a felhasználónevet és jelszót toosign a számára. **Biztosítsa, hogy hello bejelentkezési mezőben látható megadta hello URL-címen**.

10. Kattintson a hello **mentése** gombra.

11. Teheti meg, amely azt fogja automatikusan scrape URL-címet a felhasználónév és jelszó beviteli mező, és lehetővé teszik toouse az Azure AD toosecurely továbbítja a jelszavakat toothat alkalmazás hello hozzáférési panel bővítmény használatával. Hello esetében, hogy ez nem sikerül, akkor **hello bejelentkezési mód toouse manuális bejelentkezési mező rögzítési módosítása** folytatással toostep 12.

12. Kattintson a **konfigurálása &lt;appname&gt; jelszó egyszeri bejelentkezési beállítások**.

13. Jelölje be hello **manuálisan észleli a bejelentkezési mező** konfigurációs beállítást.

14. Kattintson az **OK** gombra.

15. Kattintson a **Save** (Mentés) gombra.

16. Hello kövesse a képernyőn utasításokat toouse hello hozzáférési Panel.

## <a name="i-see-a-we-couldnt-find-any-sign-in-fields-at-that-url-error"></a>A "Minden bejelentkezés mezőt, hogy az URL-címen nem található" hiba

Ezt a hibaüzenetet látja, ha a bejelentkezési mezők automatikus észlelése nem sikerült. a problémát, próbálja meg manuális bejelentkezési mező észlelését, a következő hello szükséges lépések hello tooresolve [hogyan toomanually rögzítése az alkalmazás bejelentkezési mezők](#how-to-manually-capture-sign-in-fields-for-an-application) szakasz.

## <a name="i-see-an-unable-toosave-single-sign-on-configuration-error"></a>"Nem toosave-konfiguráció egyszeri bejelentkezést." hibaüzenet

Ritka esetekben hello egyszeri bejelentkezés konfigurációjának frissítése sikertelen lehet. a hello egyszeri bejelentkezés konfigurációjának mentése újra próbálja tooresolve.

Ha ez továbbra is toofail következetesen, nyissa meg a támogatási esetet, és adja meg a hello összegyűjtött hello információt [hogyan toosee hello adatait a portál értesítései](#i-cannot-manually-detect-sign-in-fields-for-my-application) és [hogyan segíthet az tooget részletek tooa értesítés küldése támogatja a visszafejtés](#how-to-get-help-by-sending-notification-details-to-a-support-engineer) szakaszok.

## <a name="i-cannot-manually-detect-sign-in-fields-for-my-application"></a>I nem manuálisan észlel bejelentkezési mezőket az alkalmazáshoz

Előfordulhat, ha manuális észlelése nem működik látható hello viselkedésmódok közé:

-   hello manuális rögzítési folyamat toowork jelent meg, de nem volt megfelelő rögzített hello mezők

-   hello jobb mezők nem beolvasni a kijelölt hello rögzítési folyamat végrehajtása során

-   hello rögzítési folyamat veszi át me toohello alkalmazás bejelentkezési lap elvárásoknak megfelelően, de semmi nem történik

-   Kézi rögzítési toowork megjelenik, de SSO nem kerül sor, amikor a felhasználók megnyitják a hozzáférési Panel hello toohello alkalmazás.

Ha ezeket a problémákat tapasztal, ellenőrizze a hello következőket:

-   Győződjön meg arról, hogy rendelkezik-e hello hozzáférési panel bővítmény legújabb verziója hello **telepített** és **engedélyezett** hello hello utasításait követve [hogyan tooinstall hello hozzáférési Panel böngésző bővítmény](#how-to-install-the-access-panel-browser-extension) szakasz.

-   Győződjön meg arról, hogy nem kívánt hello rögzítési folyamat közben a böngésző a **incognito, inPrivate vagy privát mód**. hello hozzáférési panel bővítmény e mód nem támogatott.

-   Győződjön meg arról, hogy a felhasználók nem próbálja hello hozzáférési panelén közben toohello alkalmazásban toosign **incognito, inPrivate vagy privát mód**. hello hozzáférési panel bővítmény e mód nem támogatott.

-   Próbálja meg újból hello manuális rögzítését, piros hello jelölők teljesülnek keresztül hello megfelelő mezőket.

-   Ha hello manuális rögzítési folyamat toohang úgy tűnik, vagy hello bejelentkezési oldal nem semmit (3. eset fent), próbálja hello manuális rögzítési folyamat újra. De hello folyamat, nyomja meg az hello befejezése után most **F12** tooopen gombra a böngésző fejlesztői konzolján. Egyszer, nyissa meg a hello **konzol** és típus **window.location= "&lt;hello bejelentkezési hello app konfigurálásakor megadott URL-címet adjon meg&gt;"** , és nyomja le az  **Adja meg**. A kényszerített egy lap átirányítási hello rögzítési folyamat befejezéséhez és hello mezők rögzített kell tárolni.

Ha ezek egyike sem működik, meg, segítünk. Nyisson meg egy támogatási esetet mi próbált, valamint hello összegyűjtött hello információt hello részleteit [hogyan toosee hello adatait a portál értesítései](#i-cannot-manually-detect-sign-in-fields-for-my-application) és [hogyan segíthet az tooget értesítési adatokat küldjenek tooa támogatása a visszafejtés](#how-to-get-help-by-sending-notification-details-to-a-support-engineer) részek (ha van ilyen).

## <a name="how-tooinstall-hello-access-panel-browser-extension"></a>Hogyan tooinstall hello hozzáférési Panel bővítmény

tooinstall hello hozzáférési Panel bővítmény, kövesse az alábbi hello lépéseket:

1.  Nyissa meg hello [hozzáférési Panel](https://myapps.microsoft.com) valamelyik hello támogatott böngészők és való bejelentkezést egy **felhasználói** az Azure AD-ben.

2.  Kattintson egy **jelszó-SSO alkalmazás** a hozzáférési Panel hello.

3.  Hello Rákérdezés azzal a kérdéssel tooinstall hello szoftver, válassza ki **telepítés**.

4.  A böngésző alapján kell irányított toohello letöltési hivatkozását. **Adja hozzá** hello bővítmény tooyour böngésző.

5.  Ha a böngésző kéri, válassza ki a tooeither **engedélyezése** vagy **engedélyezése** hello bővítmény.

6.  Korábban telepítve, **indítsa újra a** a böngésző-munkamenetet.

7.  Jelentkezzen be a hozzáférési Panel hello, és tekintse meg, ha **indítása** a jelszó-egyszeri bejelentkezés alkalmazásokhoz.

Az alábbi hello közvetlen hivatkozások Chrome és Firefox is letöltheti hello bővítmény:

-   [Chrome hozzáférési Panel bővítmény](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

-   [A Firefox hozzáférési Panel bővítmény](https://addons.mozilla.org/firefox/addon/access-panel-extension/)

## <a name="how-toosee-hello-details-of-a-portal-notification"></a>Hogyan toosee hello adatait a portál értesítései

A portál értesítései hello részleteit láthatja hello alábbi lépéseket követve:

1.  Kattintson a hello **értesítések** hello hello Azure portál jobb felső sarkában (hello harang) ikonra

2.  Válassza ki az értesítésekhez egy **hiba** (piros (!) következő toothem rendelkezők) állapotát.

  >! Vegye figyelembe], nem kattintson az értesítések egy **sikeres** vagy **folyamatban lévő** állapotát.
  >
  >

3.  A nyitott hello **értesítési részletek** panelen.

4.  Ezt az információt használja saját kezűleg toounderstand több hello probléma vonatkozó részletek.

5.  Ha további segítségre van, is megoszthatja ezeket az információkat a támogatási szakértőhöz vagy hello csoport tooget Terméksúgó a probléma megoldásában.

6.  Hello kattintson **másolási** **ikon** sarkában hello toohello **másolja át a hiba** szövegmező toocopy összes hello értesítési részletek tooshare és a támogatási szolgálathoz vagy a termék csoport mérnöke.

## <a name="how-tooget-help-by-sending-notification-details-tooa-support-engineer"></a>Hogyan segíthet az tooget értesítési részletek tooa támogatási szakértőhöz küldése

Nagyon fontos, hogy megosztott **összes alább felsorolt részletek hello** , ha segítségre van szüksége, így gyorsan segít a támogatási szakértőhöz. Ehhez egyszerűen az **elkészíti a képernyőfelvételt** vagy hello kattintva **másolási hibaikon**, hello toohello jobbra található **hiba másolása** szövegmező.

## <a name="notification-details-explained"></a>Értesítési részletek alapján

az alábbi hello több milyen egyes hello értesítési elemek jelent, és azok által biztosított példák ismerteti.

### <a name="essential-notification-items"></a>Alapvető értesítési elemek

-   **Cím** – hello informatív címet hello értesítés

    -   Példa – **Application proxy beállításai**

-   **Leírás** – hello Mi történt hello művelet leírása

    -   Példa – **megadott belső URL-cím már használatban van egy másik alkalmazás.**

-   **Értesítés azonosítója** – hello értesítési hello egyedi azonosítója

    -   Példa – **clientNotification-2adbfc06-2073-4678-a69f-7eb78d96b068**

-   **Ügyfélkérelem-azonosító** – a böngésző által végrehajtott hello adott kérelem azonosítója

    -   Példa – **302fd775-3329-4670-a9f3-bea37004f0bc**

-   **Stamp UTC-idő** – hello időbélyeg időintervalluma hello értesítési történt UTC szerint

    -   Példa – **2017-03-23T19:50:43.7583681Z**

-   **Belső tranzakció azonosítója** – hello használhatjuk toolook hello hiba a rendszer belső azonosítója

    -   Példa – **71a2f329-ca29-402f-aa72-bc00a7aca603**

-   **Egyszerű felhasználónév** – hello műveletet végre hello felhasználó

    -   – Példa**tperkins@f128.info**

-   **A bérlői azonosító** – hello egyedi Azonosítóját hello olyan bérlő számára, felhasználói hello hello műveletet végrehajtó tagja volt.

    -   Példa – **7918d4b5-0442-4a97-be2d-36f9f9962ece**

-   **Felhasználói objektum azonosítója** – hello hello műveletet végre hello felhasználó egyedi azonosítója

    -   Példa – **17f84be4-51f8-483a-b533-383791227a99**

### <a name="detailed-notification-items"></a>Részletes értesítési elemek

-   **Megjelenítendő név** – **(üres is lehet)** hello hiba részletes megjelenítendő neve

    -   Példa * – **Application proxy beállításai**

-   **Állapot** – hello hello értesítési adott állapota

    -   Példa * – **nem sikerült**

-   **Objektumazonosító:** – **(üres is lehet)** hello Objektumazonosító alapján mely hello művelet végrehajtása

    -   Példa – **8e08161d-f2fd-40ad-a34a-a9632d6bb599**

-   **Részletek** – hello részletes Mi történt hello művelet leírása

    -   Példa – **belső URL-cím "http://bing.com/" érvénytelen, mert már használatban van**

-   **Másolja át a hiba** – hello kattintson **másolás ikon** sarkában hello toohello **hiba másolása** szövegmező toocopy összes hello értesítési részletek tooshare a támogatási szolgálathoz vagy a termék csoport visszafejtés

    -   – Példa```{"errorCode":"InternalUrl\_Duplicate","localizedErrorDetails":{"errorDetail":"Internal url 'http://google.com/' is invalid since it is already in use"},"operationResults":\[{"objectId":null,"displayName":null,"status":0,"details":"Internal url 'http://bing.com/' is invalid since it is already in use"}\],"timeStampUtc":"2017-03-23T19:50:26.465743Z","clientRequestId":"302fd775-3329-4670-a9f3-bea37004f0bb","internalTransactionId":"ea5b5475-03b9-4f08-8e95-bbb11289ab65","upn":"tperkins@f128.info","tenantId":"7918d4b5-0442-4a97-be2d-36f9f9962ece","userObjectId":"17f84be4-51f8-483a-b533-383791227a99"}```

## <a name="next-steps"></a>Következő lépések
[Adja meg az egyszeri bejelentkezés tooyour alkalmazások alkalmazásproxyval](active-directory-application-proxy-sso-using-kcd.md)

