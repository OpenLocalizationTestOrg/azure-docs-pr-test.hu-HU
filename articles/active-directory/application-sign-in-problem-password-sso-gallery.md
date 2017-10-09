---
title: "bejelentkezés tooan konfigurált összevont Azure AD-katalógusában alkalmazás aaaProblems egyszeri bejelentkezés |} Microsoft Docs"
description: "Hogyan tootroubleshoot problémák jelszó egyszeri bejelentkezés beállítása az Azure AD-gyűjtemény alkalmazással"
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
ms.openlocfilehash: 7ba28765e1d1f13025d740790a2f87654ae0daed
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="problems-signing-in-tooan-azure-ad-gallery-application-configured-for-federated-single-sign-on"></a>Bejelentkezés az összevont egyszeri bejelentkezés beállítása az Azure AD-gyűjtemény alkalmazás tooan problémák

Hozzáférési Panel hello egy webes portál, mely lehetővé teszi, hogy egy felhasználó, aki rendelkezik a munkahelyi vagy iskolai fiókot az Azure Active Directory (Azure AD) tooview, és indítsa el felhőalapú alkalmazások, hogy az Azure AD hello rendszergazda engedélyezte őket a hozzáférést. Egy felhasználó, aki rendelkezik az Azure AD-verziók is használhatja, önkiszolgáló csoportkezelési és a hozzáférési Panel hello keresztül kezelési képességeit. Hozzáférési Panel hello elkülönül hello Azure-portálon, és nem igényli a felhasználók toohave Azure-előfizetéssel.

toouse jelszó-alapú egyszeri bejelentkezést (SSO) a hozzáférési Panel, a hozzáférési Panel bővítmény hello hello telepíteni kell hello felhasználó böngészőjében. A bővítmény le automatikusan, ha a felhasználó megadja egy alkalmazás, amely jelszóalapú SSO van konfigurálva.

## <a name="meeting-browser-requirements-for-hello-access-panel"></a>A hozzáférési Panel hello értekezlet böngészőre vonatkozó követelményei

hello hozzáférési Panel igényel, amely támogatja a JavaScript egy böngészőt, és CSS engedélyezte. toouse jelszó-alapú egyszeri bejelentkezést (SSO) a hozzáférési Panel, a hozzáférési Panel bővítmény hello hello telepíteni kell hello felhasználó böngészőjében. A bővítmény le automatikusan, ha a felhasználó megadja egy alkalmazás, amely jelszóalapú SSO van konfigurálva.

Az egyszeri bejelentkezés jelszóalapú hello végfelhasználó böngészőkkel lehet:

-   Internet Explorer 8, 9, 10, 11 - Windows 7 vagy újabb

-   Chrome - Windows 7 vagy újabb, és MacOS X rendszeren vagy újabb

-   Firefox 26.0 vagy később - a Windows XP SP2 vagy újabb, és a Mac OS X 10,6 vagy újabb verzió

>[!NOTE]
>hello jelszóalapú SSO bővítmény a Windows 10-es szegély rendelkezésre állására böngészőbővítményeket peremhálózati lesz támogatott.
>
>

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

## <a name="setting-up-a-group-policy-for-internet-explorer"></a>A csoportházirend beállítása az Internet Explorerben

Beállíthatja a csoportházirend, amelyek lehetővé teszik tooremotely telepítés hello hozzáférési Panel bővítményt az Internet Explorer a felhasználók gépeken.

hello Előfeltételek a következők:

-   Ezzel beállította [Active Directory tartományi szolgáltatások](https://msdn.microsoft.com/library/aa362244%28v=vs.85%29.aspx), és a felhasználók gépek tooyour tartományhoz csatlakozott.

-   Hello "Beállítások módosítása" engedéllyel tooedit hello csoportházirend-objektumot (GPO) kell rendelkeznie. Alapértelmezés szerint a következő biztonsági csoportokat hello tagjai rendelkezik ilyen engedéllyel: tartományi rendszergazdák, a vállalati rendszergazdák és a Csoportházirend-létrehozó tulajdonosok. [További információk](https://technet.microsoft.com/library/cc781991%28v=ws.10%29.aspx).

Hello az oktatóanyagot követve [hogyan tooDeploy hello csoportházirend használatával az Internet Explorer hozzáférési Panel bővítmény](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ie-group-policy) hogyan tooconfigure hello csoportházirend, majd központilag telepítenie toousers részletes információkra van szüksége.

## <a name="troubleshoot-hello-access-panel-in-internet-explorer"></a>Az Internet Explorerben a hozzáférési Panel hello hibaelhárítása

Hajtsa végre a hello [kapcsolatos problémák elhárítása hello hozzáférési Panel bővítményét az Internet Explorer](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ie-Troubleshoot) útmutatót a hozzáféréshez olyan diagnosztikai eszköz és részletes utasításokat az hello bővítmény konfigurálása az Internet Explorer.

## <a name="how-tooconfigure-password-single-sign-on-for-an-azure-ad-gallery-application"></a>Hogyan tooconfigure jelszó egyszeri bejelentkezés az Azure AD gyűjtemény alkalmazáshoz

egy alkalmazás hello Azure AD-galériából tooconfigure kell:

-   [Alkalmazás hozzáadása hello Azure AD-galériából](#_Add_an_application)

-   [Állítsa be hello alkalmazását jelszó egyszeri bejelentkezést.](#configure-the-application-for-password-single-sign-on)

-   [Felhasználók hozzárendelése toohello alkalmazás](#assign-users-to-the-application)

### <a name="add-an-application-from-hello-azure-ad-gallery"></a>Alkalmazás hozzáadása hello Azure AD-galériából

az alkalmazás a hello Azure AD-katalógusában, tooadd kövesse hello alábbi lépéseket:

1.  Nyissa meg hello [Azure Portal](https://portal.azure.com) , és jelentkezzen be a egy **globális rendszergazda** vagy **társadminisztrátornak**

2.  Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.

3.  Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.

4.  Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.

5.  Kattintson a hello **hozzáadása** hello jobb felső sarokban lévő hello a gomb **vállalati alkalmazások** panelen.

6.  A hello **adjon meg egy nevet** hello a szövegmező **hello gyűjteményből Hozzáadás** szakaszban, hello alkalmazás hello nevét.

7.  Válassza ki a használni kívánt tooconfigure egyszeri bejelentkezés hello alkalmazást.

8.  Hello alkalmazás hozzáadása előtt módosíthatja annak nevét a hello **neve** szövegmező.

9.  Kattintson a **Hozzáadás** tooadd hello alkalmazás gomb.

Egy rövid időszak után kell képes toosee hello alkalmazás konfigurációs panelen.

### <a name="configure-hello-application-for-password-single-sign-on"></a>Állítsa be hello alkalmazását jelszó egyszeri bejelentkezést.

tooconfigure egyszeri bejelentkezés egy alkalmazás kövesse az alábbi hello lépéseket:

1.  Nyitott hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**

2.  Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.

3.  Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.

4.  Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.

5.  Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.

   * Ha hello alkalmazás jelenik itt meg nem látja, akkor hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját** és set hello **megjelenítése** beállítás túl **Minden alkalmazást.**

6.  Válassza ki a kívánt tooconfigure egyszeri bejelentkezés hello alkalmazás

7.  Ha hello alkalmazás betölt, kattintson a hello **egyszeri bejelentkezés** hello alkalmazás bal oldali navigációs menüjében.

8.  Jelölje be hello mód **jelszóalapú bejelentkezés.**

9.  [Felhasználók hozzárendelése toohello alkalmazás](#_How_to_assign).

10. Emellett is megadhatja hello felhasználó nevében hitelesítő adatok hello felhasználók hello sorát kiválasztásával, és kattintson a **frissítéséhez szükséges hitelesítő adatokat** és hello felhasználónév és jelszó megadásával hello felhasználók nevében. Ellenkező esetben a felhasználók fognak felszólító tooenter hello hitelesítő adatok magukat az indítás után.

### <a name="assign-users-toohello-application"></a>Felhasználók hozzárendelése toohello alkalmazás

tooassign felhasználók tooan egy vagy több alkalmazás közvetlenül, kövesse az alábbi hello lépéseket:

1.  Nyissa meg hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be a egy **globális rendszergazdája.**

2.  Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.

3.  Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.

4.  Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.

5.  Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.

   * Ha hello alkalmazás jelenik itt meg nem látja, akkor hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját** és set hello **megjelenítése** beállítás túl **Minden alkalmazást.**

6.  Válassza ki a kívánt felhasználó toofrom hello listáját tooassign hello alkalmazást.

7.  Amikor hello alkalmazás betölt, kattintson a **felhasználók és csoportok** hello alkalmazás bal oldali navigációs menüjében.

8.  Hello kattintson **Hozzáadás** gomb felett hello **felhasználók és csoportok** lista tooopen hello **hozzáadása hozzárendelés** panelen.

9.  hello kattintson **felhasználók és csoportok** hello a választó **hozzáadása hozzárendelés** panelen.

10. Hello típusának **teljes név** vagy **e-mail cím** érdekli hello való hozzárendelése hello felhasználó **Keresés név vagy e-mail cím alapján** keresőmezőbe.

11. Hello rámutat **felhasználói** a hello lista tooreveal egy **jelölőnégyzet**. Kattintson a hello jelölőnégyzet következő toohello felhasználó profil fénykép vagy embléma tooadd a felhasználó toohello **kijelölt** listája.

12. **Választható lehetőség:** Ha túl szeretné**egynél több felhasználó hozzáadása**, egy másik típus **teljes név** vagy **e-mail cím** be hello **Keresés név e-mail cím vagy** keresési mezőbe, majd kattintson a hello jelölőnégyzet tooadd a felhasználó toohello **kijelölt** listája.

13. Ha elkészült, válassza a felhasználók, kattintson a hello **válasszon** gomb tooadd őket felhasználók és csoportok toobe toohello listája toohello alkalmazás hozzárendelése.

14. **Nem kötelező:** hello kattintson **Szerepkörválasztás** hello a választó **hozzáadása hozzárendelés** panel tooselect szerepkör tooassign toohello felhasználók kijelölt.

15. Kattintson a hello **hozzárendelése** gomb tooassign hello alkalmazás toohello kijelölt felhasználók.

Miután rövid időn belül, kijelölt hello felhasználók kell tudni toolaunch ezeket az alkalmazásokat hello a hozzáférési Panel.

## <a name="if-these-troubleshoot-steps-dont-resolve-hello-issue"></a>Ha ezek elhárításához a lépést nem sikerül megoldani a hello probléma 
Nyisson meg egy támogatási jegy hello ha rendelkezésre áll a következő információkat:

-   Megfelelési hiba azonosítója

-   Egyszerű felhasználónév (felhasználó e-mail címe)

-   A TenantID

-   Böngésző típusa

-   Időzóna és idő/időkeretre során hiba történik.

-   Fiddler nyomkövetések

## <a name="next-steps"></a>Következő lépések
[Adja meg az egyszeri bejelentkezés tooyour alkalmazások alkalmazásproxyval](active-directory-application-proxy-sso-using-kcd.md)
