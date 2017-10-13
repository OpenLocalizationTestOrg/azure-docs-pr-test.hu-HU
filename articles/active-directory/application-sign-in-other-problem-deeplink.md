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
# <a name="problems-signing-in-to-an-application-using-a-deeplink"></a>A mélyhivatkozási használó alkalmazások történő bejelentkezés problémák

A hozzáférési Panel egy webes portál, amely lehetővé teszi a felhasználó munkahelyi vagy iskolai fiókkal az Azure Active Directory (Azure AD) a megtekintése, és indítsa el a felhőalapú alkalmazások, hogy az Azure AD-rendszergazda engedélyezte őket hozzáférés annak. 

Ezeket az alkalmazásokat úgy vannak konfigurálva, az Azure AD portálon a felhasználó nevében. Az alkalmazás megfelelően konfigurálva legyen, és a felhasználó vagy egy csoport, a felhasználó tagja a hozzáférési panelen az alkalmazást.

Mélyhivatkozással vagy a felhasználói hozzáférés URL-címei a felhasználók használhatják a jelszó-SSO alkalmazások közvetlenül elérje a böngészők URL-cím sávok hivatkozásokat. Nyissa meg ezt a hivatkozást, a felhasználók fognak automatikusan nélkül a hozzáférési panel először az alkalmazás aláírt. Ez az egyazon kapcsolat, amely felhasználói az Office 365 alkalmazásindító ezek az alkalmazások eléréséhez.

## <a name="general-issues-to-check-first"></a>Először ellenőrizze a általános problémák

-   Győződjön meg arról, hogy a használatával egy **böngésző** , amely megfelel a hozzáférési Panel minimális követelményeinek.

-   Győződjön meg arról, hogy a felhasználó böngészőjéből az alkalmazás URL-CÍMÉT hozzáadta a **megbízható helyek**.

-   Ellenőrizze, hogy az alkalmazás **konfigurált** megfelelően.

-   Győződjön meg arról, hogy a felhasználói fiók **engedélyezett** a indított bejelentkezések.

-   Győződjön meg arról, hogy a felhasználói fiók **nincs zárolva.**

-   Győződjön meg arról, hogy a felhasználó **nem lejárt vagy elfelejtett jelszó.**

-   Győződjön meg arról, hogy **multi-factor Authentication** nem blokkolja a hozzáférést.

-   Győződjön meg arról, hogy egy **feltételes hozzáférési házirend** vagy **Identity Protection** házirend nem blokkolja a hozzáférést.

-   Győződjön meg arról, hogy a felhasználó **hitelesítési kapcsolattartási adatai** esetén a multi-factor Authentication vagy feltételes hozzáférési házirendek kényszerítését dátum.

-   A győződjön meg arról is megpróbálja törlésével a böngésző cookie-kat, majd próbálkozzon újból bejelentkezni.

## <a name="checking-the-deeplink"></a>A mélyhivatkozási ellenőrzése

Ellenőrizze, hogy van-e a megfelelő mélyhivatkozás, kövesse az alábbi lépéseket:

1.  Nyissa meg a [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**

2.  Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.

3.  Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.

4.  Kattintson a **vállalati alkalmazások** az Azure Active Directory bal oldali navigációs menüjében.

5.  Kattintson a **összes alkalmazás** az alkalmazások listájának megtekintéséhez.

  * Ha azt szeretné, hogy itt megjelennek az alkalmazás nem látja, használja a **szűrő** vezérlő tetején a **összes alkalmazások listáját** és állítsa be a **megjelenítése** lehetőséggel **összes alkalmazást.**

6.  Nyissa meg a [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**

7.  Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.

8.  Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.

9.  Kattintson a **vállalati alkalmazások** az Azure Active Directory bal oldali navigációs menüjében.

10. Kattintson a **összes alkalmazás** az alkalmazások listájának megtekintéséhez.

   * Ha azt szeretné, hogy itt megjelennek az alkalmazás nem látja, használja a **szűrő** vezérlő tetején a **összes alkalmazások listáját** és állítsa be a **megjelenítése** lehetőséggel **összes alkalmazást.**

11. A mélyhivatkozási az ellenőrzés kívánt alkalmazás kiválasztása.

12. A címke található **felhasználói URL-CÍMEN**. Ön mélyhivatkozás URL-címet meg kell felelnie.

## <a name="how-to-install-the-access-panel-browser-extension"></a>A hozzáférési Panel bővítmény telepítése

A hozzáférési Panel bővítmény telepítéséhez kövesse az alábbi lépéseket:

1.  Nyissa meg a [hozzáférési Panel](https://myapps.microsoft.com) valamelyik támogatott böngészők és való bejelentkezést egy **felhasználói** az Azure AD-ben.

2.  Kattintson egy **jelszó-SSO alkalmazás** a hozzáférési panelen.

3.  Válassza ki a szoftver telepítéséhez megkérdezi **telepítés**.

4.  A böngésző alapján kell irányítani a letöltési hivatkozás. **Adja hozzá** az bővítményt, hogy a böngészőben.

5.  Ha a böngésző, válassza ki vagy **engedélyezése** vagy **engedélyezése** a bővítményt.

6.  Korábban telepítve, **indítsa újra a** a böngésző-munkamenetet.

7.  Jelentkezzen be a hozzáférési panelre, és tekintse meg, ha **indítása** a jelszó-egyszeri bejelentkezés alkalmazásokhoz

Az alábbi közvetlen hivatkozások közül a Chrome és Firefox is letöltheti a bővítmény:

-   [Chrome hozzáférési Panel bővítmény](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

-   [A Firefox hozzáférési Panel bővítmény](https://addons.mozilla.org/firefox/addon/access-panel-extension/)

## <a name="how-to-configure-password-single-sign-on-for-an-azure-ad-gallery-application"></a>Jelszó egyszeri bejelentkezés az Azure AD gyűjtemény alkalmazás konfigurálása

Egy alkalmazás kell az Azure AD-galériából konfigurálása:

-   [Alkalmazás hozzáadása az Azure AD-galériából](#add-an-application-from-the-Azure-AD-gallery)

-   [Állítsa be az alkalmazását jelszó egyszeri bejelentkezést.](#configure-the-application-for-password-single-sign-on)

### <a name="add-an-application-from-the-azure-ad-gallery"></a>Alkalmazás hozzáadása az Azure AD-galériából

Hozzáadhat egy alkalmazást az Azure AD-gyűjteményből, kövesse az alábbi lépéseket:

1.  Nyissa meg a [Azure Portal](https://portal.azure.com) , és jelentkezzen be egy **globális rendszergazda** vagy **társadminisztrátor**.

2.  Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.

3.  Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.

4.  Kattintson a **vállalati alkalmazások** az Azure Active Directory bal oldali navigációs menüjében.

5.  Kattintson a **Hozzáadás** gombra a jobb felső sarkában a **vállalati alkalmazások** panelen.

6.  Az a **adjon meg egy nevet** a szövegmező a **hozzáadása a gyűjteményből** területen írja be az alkalmazás nevét.

7.  Válassza ki az alkalmazást, az egyszeri bejelentkezés konfigurálni szeretné.

8.  Ad hozzá az alkalmazást, mielőtt a nevét módosíthatja a **neve** szövegmező.

9.  Kattintson a **Hozzáadás** gombra, vegye fel az alkalmazást.

Egy rövid időszak után el az alkalmazás konfigurációs panelen láthatók.

### <a name="configure-the-application-for-password-single-sign-on"></a>Állítsa be az alkalmazását jelszó egyszeri bejelentkezést.

Egyszeri bejelentkezés egy alkalmazás konfigurálásához kövesse az alábbi lépéseket:

1.  Nyissa meg a [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**

2.  Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.

3.  Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.

4.  Kattintson a **vállalati alkalmazások** az Azure Active Directory bal oldali navigációs menüjében.

5.  Kattintson a **összes alkalmazás** az alkalmazások listájának megtekintéséhez.

  * Ha azt szeretné, hogy itt megjelennek az alkalmazás nem látja, használja a **szűrő** vezérlő tetején a **összes alkalmazások listáját** és állítsa be a **megjelenítése** lehetőséggel **összes alkalmazást.**

6.  Válassza ki az egyszeri bejelentkezés konfigurálni kívánt alkalmazást.

7.  Ha az alkalmazás betölt, kattintson a **egyszeri bejelentkezés** az alkalmazás bal oldali navigációs menüjében.

8.  Válassza ki a módot **jelszóalapú bejelentkezés.**

9.  [Felhasználók hozzárendelése az alkalmazás](#how-to-assign-a-user-to-an-application-directly).

10. Emellett is megadhatja a felhasználó nevében hitelesítő adatok a felhasználók a sorok kijelöléséhez és parancsával **frissítéséhez szükséges hitelesítő adatokat** , majd gépelje be a felhasználónevet és jelszót a felhasználók nevében. Ellenkező esetben megkérdezi a felhasználókat a hitelesítő adatok magukat az indítás után.

## <a name="how-to-configure-password-single-sign-on-for-a-non-gallery-application"></a>Jelszó egyszeri bejelentkezés nem galéria alkalmazások konfigurálása

Egy alkalmazás kell az Azure AD-galériából konfigurálása:

-   [Nem galéria alkalmazás hozzáadása](#add-a-non-gallery-application)

-   [Állítsa be az alkalmazását jelszó egyszeri bejelentkezést.](#configure-the-application-for-password-single-sign-on)

### <a name="add-a-non-gallery-application"></a>Nem galéria alkalmazás hozzáadása

Hozzáadhat egy alkalmazást az Azure AD-gyűjteményből, kövesse az alábbi lépéseket:

1.  Nyissa meg a [Azure Portal](https://portal.azure.com) , és jelentkezzen be egy **globális rendszergazda** vagy **társadminisztrátor**.

2.  Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.

3.  Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.

4.  Kattintson a **vállalati alkalmazások** az Azure Active Directory bal oldali navigációs menüjében.

5.  Kattintson a **Hozzáadás** gombra a jobb felső sarkában a **vállalati alkalmazások** panelen.

6.  Kattintson a **nem-gyűjtemény alkalmazás.**

7.  Adja meg az alkalmazás nevét a **neve** szövegmező. Válassza ki **hozzáadása.**

Egy rövid időszak után el az alkalmazás konfigurációs panelen láthatók.

### <a name="configure-the-application-for-password-single-sign-on"></a>Állítsa be az alkalmazását jelszó egyszeri bejelentkezést.

Egyszeri bejelentkezés egy alkalmazás konfigurálásához kövesse az alábbi lépéseket:

1.  Nyissa meg a [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**

2.  Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.

3.  Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.

4.  Kattintson a **vállalati alkalmazások** az Azure Active Directory bal oldali navigációs menüjében.

5.  Kattintson a **összes alkalmazás** az alkalmazások listájának megtekintéséhez.

    1.  Ha azt szeretné, hogy itt megjelennek az alkalmazás nem látja, használja a **szűrő** vezérlő tetején a **összes alkalmazások listáját** és állítsa be a **megjelenítése** lehetőséggel **összes alkalmazást.**

6.  Válassza ki az egyszeri bejelentkezés konfigurálni kívánt alkalmazást.

7.  Ha az alkalmazás betölt, kattintson a **egyszeri bejelentkezés** az alkalmazás bal oldali navigációs menüjében.

8.  Válassza ki a módot **jelszóalapú bejelentkezés.**

9.  Adja meg a **bejelentkezési URL-cím**. Ez az URL-CÍMÉT ahol adja meg a felhasználók a felhasználónevével és jelszavával bejelentkezni. Győződjön meg arról, a mezők bejelentkezési URL-címen láthatók.

10. Felhasználók hozzárendelése az alkalmazáshoz.

11. Emellett is megadhatja a felhasználó nevében hitelesítő adatok a felhasználók a sorok kijelöléséhez és parancsával **frissítéséhez szükséges hitelesítő adatokat** , majd gépelje be a felhasználónevet és jelszót a felhasználók nevében. Ellenkező esetben megkérdezi a felhasználókat a hitelesítő adatok magukat az indítás után.

## <a name="how-to-assign-a-user-to-an-application-directly"></a>A felhasználó közvetlenül egy alkalmazás hozzárendelése

Hozzárendelése egy vagy több felhasználó alkalmazás közvetlenül, kövesse az alábbi lépéseket:

1.  Nyissa meg a [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazdája.**

2.  Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.

3.  Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.

4.  Kattintson a **vállalati alkalmazások** az Azure Active Directory bal oldali navigációs menüjében.

5.  Kattintson a **összes alkalmazás** az alkalmazások listájának megtekintéséhez.

  * Ha azt szeretné, hogy itt megjelennek az alkalmazás nem látja, használja a **szűrő** vezérlő tetején a **összes alkalmazások listáját** és állítsa be a **megjelenítése** lehetőséggel **összes alkalmazást.**

6.  Válassza ki szeretné osztani a felhasználót, hogy a listában az alkalmazást.

7.  Ha az alkalmazás betölt, kattintson **felhasználók és csoportok** az alkalmazás bal oldali navigációs menüjében.

8.  Kattintson a **Hozzáadás** gombra kattint, a a **felhasználók és csoportok** nyissa meg a listában a **hozzáadása hozzárendelés** panelen.

9.  Kattintson a **felhasználók és csoportok** a választó a **hozzáadása hozzárendelés** panelen.

10. Írja be a **teljes név** vagy **e-mail cím** érdekli hozzárendelése a felhasználó a **Keresés név vagy e-mail cím alapján** keresőmezőbe.

11. Vigye a **felhasználói** a listában, hogy láthatóvá váljon a **jelölőnégyzet**. A felhasználói profil fénykép vagy adja hozzá a felhasználót emblémát jelölőnégyzetét, kattintson a **kijelölt** listája.

12. **Választható lehetőség:** Ha azt szeretné, hogy **egynél több felhasználó hozzáadása**, egy másik típus **teljes név** vagy **e-mail cím** be a **Keresés név vagy e-mail cím alapján** mező, és a jelölőnégyzet bejelölésével adja hozzá a felhasználót, hogy a **kijelölt** listája.

13. Ha elkészült, válassza a felhasználók, kattintson a **válasszon** gombra kattintva vegye fel a listára a felhasználók és csoportok hozzá kell rendelni az alkalmazáshoz.

14. **Választható lehetőség:** kattintson a **Szerepkörválasztás** a választó a **hozzáadása hozzárendelés** hozzárendelése a kiválasztott felhasználói szerepkör kiválasztása panel.

15. Kattintson a **hozzárendelése** gombra kattintva a kijelölt felhasználók az alkalmazást.

Rövid ideig a kijelölt felhasználók tudják elindítani ezeket az alkalmazásokat a hozzáférési Panel.

## <a name="if-these-troubleshooting-steps-do-not-the-resolve-the-issue"></a>Ha ezek a hibaelhárítási lépéseket nem a hárítsa el a problémát. 

támogatási jegy megnyitása a következő információkat, ha rendelkezésre áll:

-   Megfelelési hiba azonosítója

-   Egyszerű felhasználónév (felhasználó e-mail címe)

-   A TenantID

-   Böngésző típusa

-   Időzóna és idő/időkeretre során hiba történik.

-   Fiddler nyomkövetések

## <a name="next-steps"></a>Következő lépések
[Adja meg az egyszeri bejelentkezés az alkalmazásokba a Proxy](active-directory-application-proxy-sso-using-kcd.md)
