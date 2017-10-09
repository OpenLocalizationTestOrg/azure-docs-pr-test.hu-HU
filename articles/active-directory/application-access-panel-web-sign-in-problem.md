---
title: "bejelentkezés toohello hozzáférési panel webhely aaaProblem |} Microsoft Docs"
description: "Útmutatás tootroubleshoot problémák merülhetnek fel a toouse toosign közben hello hozzáférési Panel"
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
ms.reviwer: japere
ms.openlocfilehash: 1037f7c5fbaa9425760ad5739b383c716d5fc3a3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="problem-signing-in-toohello-access-panel-website"></a>Bejelentkezés toohello hozzáférési panel webhelyen

Hozzáférési Panel hello egy webes portál, mely lehetővé teszi, hogy egy felhasználó, aki rendelkezik a munkahelyi vagy iskolai fiókot az Azure Active Directory (Azure AD) tooview, és indítsa el felhőalapú alkalmazások, hogy az Azure AD hello rendszergazda engedélyezte őket a hozzáférést. Egy felhasználó, aki rendelkezik az Azure AD-verziók is használhatja, önkiszolgáló csoportkezelési és a hozzáférési Panel hello keresztül kezelési képességeit. Hozzáférési Panel hello elkülönül hello Azure-portálon, és nem igényli a felhasználók toohave Azure-előfizetéssel.

Ha munkahelyi vagy iskolai fiókkal az Azure AD hozzáférési Panel toohello felhasználók bejelentkezhetnek.

-   Felhasználók hitelesítése közvetlenül az Azure ad.

-   Felhasználók hitelesítése az Active Directory összevonási szolgáltatások (AD FS) használatával.

-   Windows Server Active Directory felhasználók hitelesíthetők.

Ha a felhasználó rendelkezik előfizetéssel az Azure vagy Office 365, és hello Azure-portálon vagy az Office 365 alkalmazást használ, képes toouse lesz a toosign újra anélkül zökkenőmentesen hello a hozzáférési Panel. Nem hitelesített felhasználók a kért toosign kell használatával hello felhasználónevet és jelszót a fiókjuk Azure AD-ben. Ha hello szervezet konfigurálva van összevonási, írja be a hello felhasználónév is használhatók.

## <a name="general-issues-toocheck-first"></a>Általános problémák első toocheck 

-   Ellenőrizze, hogy hello felhasználói bejelentkezéskor van toohello **javítsa ki az URL-cím**: <https://myapps.microsoft.com>

-   Ellenőrizze, hogy hello felhasználó böngészője által hozzáadott hello URL-cím tooits **megbízható helyek**

-   Ellenőrizze, hogy a hello felhasználói fiók **engedélyezett** a indított bejelentkezések.

-   Ellenőrizze, hogy a hello felhasználói fiók **nincs zárolva.**

-   Győződjön meg arról, hogy hello felhasználói **nem lejárt vagy elfelejtett jelszó.**

-   Győződjön meg arról, hogy **multi-factor Authentication** nem blokkolja a hozzáférést.

-   Győződjön meg arról, hogy egy **feltételes hozzáférési házirend** vagy **Identity Protection** házirend nem blokkolja a hozzáférést.

-   Győződjön meg arról, hogy a felhasználó **hitelesítési kapcsolattartási adatai** működik-e toodate tooallow többtényezős hitelesítést vagy feltételes hozzáférési házirendek toobe lépnek érvénybe.

-   Győződjön meg arról, hogy tooalso próbálkozzon a böngésző cookie-k törölje, majd próbálkozzon újra a toosign.

## <a name="meeting-browser-requirements-for-hello-access-panel"></a>A hozzáférési Panel hello értekezlet böngészőre vonatkozó követelményei

hello hozzáférési Panel igényel, amely támogatja a JavaScript egy böngészőt, és CSS engedélyezte. toouse jelszó-alapú egyszeri bejelentkezést (SSO) a hozzáférési Panel, a hozzáférési Panel bővítmény hello hello telepíteni kell hello felhasználó böngészőjében. A bővítmény le automatikusan, ha a felhasználó megadja egy alkalmazás, amely jelszóalapú SSO van konfigurálva.

Az egyszeri bejelentkezés jelszóalapú hello végfelhasználó böngészőkkel lehet:

-   Internet Explorer 8, 9, 10, 11 – a Windows 7 vagy újabb

-   Peremhálózati Windows 10 évforduló Edition vagy újabb 

-   Chrome – A Windows 7 vagy újabb, és MacOS X rendszeren vagy újabb

-   Firefox 26.0 vagy újabb – a Windows XP SP2 vagy újabb, és a Mac OS X 10,6 vagy újabb verzió


## <a name="problems-with-hello-users-account"></a>Hello felhasználói fiókkal kapcsolatos problémák

Hozzáférés toohello hozzáférési Panel blokkolható hello felhasználói fiókkal tooa probléma miatt. Az alábbiakban néhány módszert hibákat, és a felhasználó és a fiók beállításainak problémáinak megoldásával:

-   [Ellenőrizze, hogy egy felhasználói fiók létezik-e az Azure Active Directoryban](#check-if-a-user-account-exists-in-azure-active-directory)

-   [A felhasználói fiók állapotának ellenőrzése](#check-a-users-account-status)

-   [A jelszó alaphelyzetbe állítása](#reset-a-users-password)

-   [Önkiszolgáló jelszóátállítás engedélyezése](#enable-self-service-password-reset)

-   [A felhasználó a multi-factor authentication állapotának ellenőrzése](#check-a-users-multi-factor-authentication-status)

-   [Ellenőrizze a felhasználó hitelesítési kapcsolattartási adatai](#check-a-users-authentication-contact-info)

-   [A felhasználói csoporttagság ellenőrzése](#check-a-users-group-memberships)

-   [A felhasználó licenc-hozzárendeléseket ellenőrzése](#check-a-users-assigned-licenses)

-   [A felhasználó a licenc hozzárendelése](#assign-a-user-a-license)

### <a name="check-if-a-user-account-exists-in-azure-active-directory"></a>Ellenőrizze, hogy egy felhasználói fiók létezik-e az Azure Active Directoryban

toocheck, ha egy felhasználói fiók található, kövesse a hello alábbi lépéseket:

1.  Nyissa meg hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be a egy **globális rendszergazdája.**

2.  Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.

3.  Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.

4.  Kattintson a **felhasználók és csoportok** hello navigációs menüjében.

5.  Kattintson a **minden felhasználó**.

6.  **Keresési** hello felhasználó érdekli, és **hello sorban kattintson** tooselect.

7.  Ellenőrizze a hello tulajdonságainak hello felhasználói objektum toobe meg arról, hogy azok meg, mint a várt, és nincs adat hiányzik.

### <a name="check-a-users-account-status"></a>A felhasználói fiók állapotának ellenőrzése

toocheck egy felhasználói fiók állapota, kövesse az alábbi hello lépéseket:

1.  Nyissa meg hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be a egy **globális rendszergazdája.**

2.  Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.

3.  Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.

4.  Kattintson a **felhasználók és csoportok** hello navigációs menüjében.

5.  Kattintson a **minden felhasználó**.

6.  **Keresési** hello felhasználó érdekli, és **hello sorban kattintson** tooselect.

7.  Kattintson a **profil**.

8.  A **beállítások** ügyeljen arra, hogy **blokk bejelentkezés** értéke túl**nem**.

### <a name="reset-a-users-password"></a>A jelszó alaphelyzetbe állítása

tooreset egy felhasználó jelszavát, hajtsa végre hello alábbi lépéseket:

1.  Nyissa meg hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be a egy **globális rendszergazdája.**

2.  Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.

3.  Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.

4.  Kattintson a **felhasználók és csoportok** hello navigációs menüjében.

5.  Kattintson a **minden felhasználó**.

6.  **Keresési** hello felhasználó érdekli, és **hello sorban kattintson** tooselect.

7.  Kattintson a hello **jelszó-átállítási** hello felhasználói panel felső hello gombra.

8.  Kattintson a hello **jelszó-átállítási** hello gombjára **jelszó-átállítási** megjelenő panelen.

9.  Másolás hello **ideiglenes jelszó** vagy **adjon meg egy új jelszót** hello felhasználó számára.

10. Az új jelszó toohello felhasználói kommunikációhoz, ezt a jelszót, a következő során jelentkezzen be az Active Directory tooAzure szükséges toochange legyenek.

### <a name="enable-self-service-password-reset"></a>Az önkiszolgáló jelszó-visszaállítás engedélyezése

tooenable önkiszolgáló jelszó alaphelyzetbe állítása, kövesse az alábbi hello telepítési lépéseket:

-   [Engedélyezze a felhasználók tooreset a Azure Active Directory-jelszavaikat](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-getting-started#enable-users-to-reset-their-azure-ad-passwords)

-   [Felhasználók tooreset engedélyezése vagy az Active Directory helyszíni jelszavak módosítása](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-getting-started#enable-users-to-reset-or-change-their-ad-passwords)

### <a name="check-a-users-multi-factor-authentication-status"></a>A felhasználó a multi-factor authentication állapotának ellenőrzése

a felhasználó toocheck a multi-factor Authentication hitelesítési állapot, hajtsa végre hello alábbi lépéseket:

1.  Nyissa meg hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be a egy **globális rendszergazdája.**

2.  Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.

3.  Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.

4.  Kattintson a **felhasználók és csoportok** hello navigációs menüjében.

5.  Kattintson a **minden felhasználó**.

6.  Kattintson a hello **multi-factor Authentication** hello panel felső hello gombra.

7.  Egyszer hello **multi-factor Authentication felügyeleti portál** terhelés esetén gondoskodjon arról, hogy hello **felhasználók** fülre.

8.  Keresés, szűréshez vagy rendezés hello felhasználói keresése hello azoknak a felhasználóknak.

9.  Azon felhasználók hello listájáról válassza hello felhasználói és **engedélyezése**, **tiltsa le a**, vagy **érvényesítése** többtényezős hitelesítést a kívánt módon működjenek.

   >[!NOTE]
   >Ha egy felhasználó egy **kényszerített** állapotba kerül, előfordulhat, hogy túl beállítása**letiltott** ideiglenesen toolet újra be a fiókba. Miután bekerültek vissza, majd módosíthatja állapotukra túl**engedélyezve** újra toorequire őket toore regisztrálása során a következő kapcsolattartási adatait jelentkezzen be. Másik lehetőségként a lépésekkel hello a hello [ellenőrizni kell a felhasználó hitelesítési kapcsolattartási adatait](#check-a-users-authentication-contact-info) tooverify, vagy állítsa be ezeket az adatokat a számukra.
   >
   >

### <a name="check-a-users-authentication-contact-info"></a>Ellenőrizze a felhasználó hitelesítési kapcsolattartási adatai

a felhasználó hitelesítési kapcsolattartási adatok többtényezős hitelesítést, a feltételes hozzáférés, a Identity Protection és a jelszó alaphelyzetbe állításához használt toocheck hello lépéseket kövesse:

1.  Nyissa meg hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be a egy **globális rendszergazdája.**

2.  Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.

3.  Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.

4.  Kattintson a **felhasználók és csoportok** hello navigációs menüjében.

5.  Kattintson a **minden felhasználó**.

6.  **Keresési** hello felhasználó érdekli, és **hello sorban kattintson** tooselect.

7.  Kattintson a **profil**.

8.  Görgessen lefelé, túl**hitelesítési kapcsolattartási adatai**.

9.  **Felülvizsgálati** hello adatok regisztrált hello felhasználói és a frissítési igény szerint.

### <a name="check-a-users-group-memberships"></a>A felhasználói csoporttagság ellenőrzése

toocheck egy felhasználó csoporttagságok hajtsa végre az alábbi hello lépéseket:

1.  Nyissa meg hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be a egy **globális rendszergazdája.**

2.  Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.

3.  Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.

4.  Kattintson a **felhasználók és csoportok** hello navigációs menüjében.

5.  Kattintson a **minden felhasználó**.

6.  **Keresési** hello felhasználó érdekli, és **hello sorban kattintson** tooselect.

7.  Kattintson a **csoportok** toosee, amely hello felhasználói csoportok tagja.

### <a name="check-a-users-assigned-licenses"></a>A felhasználó licenc-hozzárendeléseket ellenőrzése

toocheck a felhasználó hozzárendelt licencek, hajtsa végre hello alábbi lépéseket:

1.  Nyissa meg hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be a egy **globális rendszergazdája.**

2.  Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.

3.  Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.

4.  Kattintson a **felhasználók és csoportok** hello navigációs menüjében.

5.  Kattintson a **minden felhasználó**.

6.  **Keresési** hello felhasználó érdekli, és **hello sorban kattintson** tooselect.

7.  Kattintson a **licencek** toosee licencek hello felhasználóhoz van rendelve.

### <a name="assign-a-user-a-license"></a>A felhasználó a licenc hozzárendelése 

a licenc tooa felhasználó tooassign kövesse hello alábbi lépéseket:

1.  Nyissa meg hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be a egy **globális rendszergazdája.**

2.  Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.

3.  Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.

4.  Kattintson a **felhasználók és csoportok** hello navigációs menüjében.

5.  Kattintson a **minden felhasználó**.

6.  **Keresési** hello felhasználó érdekli, és **hello sorban kattintson** tooselect.

7.  Kattintson a **licencek** toosee licencek hello felhasználóhoz van rendelve.

8.  Kattintson a hello **hozzárendelése** gombra.

9.  Válassza ki **egy vagy több termék** választható termékek hello listája.

10. **Nem kötelező** hello kattintson **hozzárendelés beállításai** elem toogranularly rendelje hozzá a termékek. Kattintson a **Ok** amikor ez befejeződik.

11. Kattintson a hello **hozzárendelése** tooassign ezen licencek toothis felhasználói gombra.

## <a name="if-these-troubleshooting-steps-do-not-resolve-hello-issue"></a>Ha ezek a hibaelhárítási lépéseket nem oldható meg hello probléma

Nyisson meg egy támogatási jegy hello ha rendelkezésre áll a következő információkat:

-   Megfelelési hiba azonosítója

-   Egyszerű felhasználónév (felhasználó e-mail címe)

-   Bérlőazonosító

-   Böngésző típusa

-   Időzóna és idő/időkeretre során hiba történik.

-   Fiddler nyomkövetések

## <a name="next-steps"></a>Következő lépések
[Adja meg az egyszeri bejelentkezés tooyour alkalmazások alkalmazásproxyval](active-directory-application-proxy-sso-using-kcd.md)
