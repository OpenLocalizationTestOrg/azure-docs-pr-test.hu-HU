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
# <a name="problems-signing-in-tooan-application-from-hello-access-panel"></a>A hozzáférési panel hello tooan alkalmazásban problémák

hello hozzáférési Panel egy webes portál, amely lehetővé teszi a felhasználó munkahelyi vagy iskolai fiókkal az Azure Active Directory (Azure AD) tooview és kezdő felhőalapú alkalmazások, hogy hello Azure AD-rendszergazda rendelkezik hozzáféréssel őket. 

Ezeket az alkalmazásokat úgy vannak konfigurálva, hello Azure AD portálon hello felhasználó nevében. hello alkalmazás megfelelően kell konfigurálni, és a hozzárendelt toohello felhasználó vagy csoport hello felhasználó tagja a hozzáférési Panel hello toosee hello alkalmazás.

a következő kategóriák hello esik azért jelent meg a felhasználó alkalmazások hello típusa:

-   Office 365-alkalmazások

-   Összevonási-alapú egyszeri bejelentkezési modellel konfigurált a Microsoft és külső alkalmazások

-   Jelszó-alapú egyszeri bejelentkezés alkalmazásokhoz

-   A már meglévő SSO megoldásaival alkalmazások

## <a name="general-issues-toocheck-first"></a>Általános problémák első toocheck

-   Győződjön meg arról, hogy a használatával egy **böngésző** , amely megfelel a hozzáférési Panel hello hello minimális követelményeinek.

-   Ellenőrizze, hogy hello böngésző hello alkalmazás tooits hello URL-CÍMÉT hozzáadta **megbízható helyek**.

-   Ellenőrizze, hogy toocheck hello alkalmazás **konfigurált** megfelelően.

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

## <a name="how-tooinstall-hello-access-panel-browser-extension"></a>Hogyan tooinstall hello hozzáférési Panel bővítmény

tooinstall hello hozzáférési Panel bővítmény, kövesse az alábbi hello lépéseket:

1.  Nyissa meg hello [hozzáférési Panel](https://myapps.microsoft.com) valamelyik hello támogatott böngészők és való bejelentkezést egy **felhasználói** az Azure AD-ben.

2.  Kattintson egy **jelszó-SSO alkalmazás** a hozzáférési Panel hello.

3.  Hello Rákérdezés azzal a kérdéssel tooinstall hello szoftver, válassza ki **telepítés**.

4.  A böngésző alapján kell irányított toohello letöltési hivatkozását. **Adja hozzá** hello bővítmény tooyour böngésző.

5.  Ha a böngésző kéri, válassza ki a tooeither **engedélyezése** vagy **engedélyezése** hello bővítmény.

6.  Korábban telepítve, **indítsa újra a** a böngésző-munkamenetet.

7.  Jelentkezzen be a hozzáférési Panel hello, és tekintse meg, ha **indítása** a jelszó-egyszeri bejelentkezés alkalmazásokhoz

Az alábbi hello közvetlen hivatkozások Chrome és a peremhálózati is letöltheti hello bővítmény:

-   [Chrome hozzáférési Panel bővítmény](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

-   [Peremhálózati hozzáférési Panel bővítmény](https://www.microsoft.com/store/apps/9pc9sckkzk84)

## <a name="how-tooconfigure-federated-single-sign-on-for-an-azure-ad-gallery-application"></a>Hogyan tooconfigure összevont egyszeri bejelentkezés az Azure AD gyűjtemény alkalmazáshoz

Hello Azure AD-dokumentumtárban vállalati egyszeri bejelentkezésre alkalmas engedélyezve van az összes alkalmazásnak érhető el részletes oktatóanyaga. Van-e hozzáférési hello [kapcsolatos bemutatók felsorolása toointegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/) kapcsolatos részletek részletes útmutatás.

egy alkalmazás hello Azure AD-galériából tooconfigure kell:

-   [Alkalmazás hozzáadása hello Azure AD-galériából](#add-an-application)

-   [Hello alkalmazás metaadatainak értékek konfigurálása az Azure AD (bejelentkezési URL-cím, az azonosítója, válasz URL-címe)](#configure-single-sign-on-for-an-application-from-the-azure-ad-gallery)

-   [Válassza ki a felhasználói azonosítót és a felhasználói attribútumok küldött toobe toohello alkalmazás hozzáadása](#select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application)

-   [Az Azure AD-metaadatok és a tanúsítvány lekérése](#download-the-azure-ad-metadata-or-certificate)

-   [Konfigurálja az Azure AD metaadatok értékeket hello alkalmazásban (bejelentkezési URL-címet, a kibocsátó, a kijelentkezési URL-cím és a tanúsítvány)](#configure-single-sign-on-for-an-application-from-the-azure-ad-gallery)

-   [Felhasználók hozzárendelése toohello alkalmazás](#assign-users-to-the-application)

### <a name="add-an-application-from-hello-azure-ad-gallery"></a>Alkalmazás hozzáadása hello Azure AD-galériából

az alkalmazás a hello Azure AD-katalógusában, tooadd kövesse hello alábbi lépéseket:

1.  Nyissa meg hello [Azure Portal](https://portal.azure.com) , és jelentkezzen be a egy **globális rendszergazda** vagy **társadminisztrátornak**

2.  Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.

3.  Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.

4.  Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.

5.  Kattintson a hello **hozzáadása** hello jobb felső sarokban lévő hello a gomb **vállalati alkalmazások** panel

6.  A hello **adjon meg egy nevet** hello a szövegmező **hello gyűjteményből Hozzáadás** szakaszban, hello alkalmazás hello nevét.

7.  Válassza ki a használni kívánt tooconfigure egyszeri bejelentkezés hello alkalmazást.

8.  Hello alkalmazás hozzáadása előtt módosíthatja annak nevét a hello **neve** szövegmező.

9.  Kattintson a **Hozzáadás** tooadd hello alkalmazás gomb.

Egy rövid időszak után kell képes toosee hello alkalmazás konfigurációs panelen.

### <a name="configure-single-sign-on-for-an-application-from-hello-azure-ad-gallery"></a>Egyszeri bejelentkezés egy alkalmazás hello Azure AD-galériából konfigurálása

tooconfigure egyszeri bejelentkezés egy alkalmazás kövesse az alábbi hello lépéseket:

1.  <span id="_Hlk477187909" class="anchor"><span id="_Hlk477001983" class="anchor"></span></span>Nyitott hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**

2.  Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.

3.  Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.

4.  Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.

5.  Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.

  * Ha hello alkalmazás jelenik itt meg nem látja, akkor hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját** és set hello **megjelenítése** beállítás túl **Minden alkalmazást.**

6.  Válassza ki a kívánt tooconfigure egyszeri bejelentkezés hello alkalmazást.

7.  Ha hello alkalmazás betölt, kattintson a hello **egyszeri bejelentkezés** hello alkalmazás bal oldali navigációs menüjében.

8.  Válassza ki **SAML-alapú bejelentkezés** a hello **mód** legördülő menüből.

9.  Adja meg a szükséges hello értékeket **tartomány és az URL-címeket.** Ezeket az értékeket kell beszerezni hello alkalmazás gyártójának segítségét.

   1. tooconfigure hello alkalmazásra, amely a Szolgáltató által kezdeményezett egyszeri Bejelentkezést, hello bejelentkezési URL-címen egy szükséges érték. Egyes alkalmazások hello azonosító értéke is szükséges.

   2. tooconfigure hello alkalmazásra, amely a kiállító terjesztési hely által kezdeményezett egyszeri Bejelentkezést, hello válasz URL-cím szükséges érték. Egyes alkalmazások hello azonosító értéke is szükséges.

10. **Választható lehetőség:** kattintson **megjelenítése speciális URL-beállításainak** Ha azt szeretné, hogy toosee hello nem szükséges értékeket.

11. A hello **felhasználói attribútumok**, válassza ki a felhasználók a hello egyedi azonosítója hello **felhasználói azonosító** legördülő menüből.

12. **Nem kötelező:** kattintson **megtekintése és szerkesztése a más felhasználói attribútumok** tooedit hello attribútumok küldött toobe toohello alkalmazás hello SAML-jogkivonat felhasználói bejelentkezéskor.

   tooadd attribútum:

   1. Kattintson a **Hozzáadás attribútum**. Adja meg a hello **neve** és hello válassza hello **érték** hello legördülő menüből.

   2. Kattintson a **mentéséhez.** Új attribútum hello hello táblázatban láthatja.

13. Kattintson a **konfigurálása &lt;alkalmazásnév&gt;**  tooaccess dokumentáció tooconfigure egyszeri bejelentkezés hello alkalmazásban. Is hogy rendelkezik hello metadata URL-címek és a szükséges tanúsítványok toosetup SSO hello alkalmazással.

14. Kattintson a **mentése** toosave hello konfigurációs.

15. Felhasználók hozzárendelése toohello alkalmazás.

### <a name="select-user-identifier-and-add-user-attributes-toobe-sent-toohello-application"></a>Válassza ki a felhasználói azonosítót és a felhasználói attribútumok küldött toobe toohello alkalmazás hozzáadása

tooselect hello felhasználói azonosítót, vagy adja hozzá a felhasználói attribútumok, hajtsa végre az alábbi lépéseket hello:

1.  Nyitott hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**

2.  Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.

3.  Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.

4.  Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.

5.  Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.

  * Ha azt szeretné, hogy itt tooshow hello alkalmazás nem látja, használja a hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját** és set hello **megjelenítése** beállítás túl **Minden alkalmazást.**

6.  Válassza ki az egyszeri bejelentkezés konfigurált hello alkalmazást.

7.  Ha hello alkalmazás betölt, kattintson a hello **egyszeri bejelentkezés** hello alkalmazás bal oldali navigációs menüjében.

8.  A hello **felhasználói attribútumok** szakaszban jelölje be a felhasználók a hello egyedi azonosítója hello **felhasználói azonosító** legördülő menüből. hello kiválasztott beállítás toomatch hello várt értéket kell hello alkalmazás tooauthenticate hello felhasználó.

    >[!NOTE]
    >Az Azure AD válassza hello formátumban hello NameID attribútum (felhasználói azonosító) kijelölt hello érték alapján, vagy a SAML AuthRequest hello hello alkalmazás által kért formátum hello. További információt a Microsoft hello cikk [egyszeri bejelentkezés SAML protokoll](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) az hello rész NameIDPolicy.
    >
    >

9.  tooadd felhasználói attribútumok, kattintson **megtekintése és szerkesztése a más felhasználói attribútumok** tooedit hello attribútumok küldött toobe toohello alkalmazás hello SAML-jogkivonat felhasználói bejelentkezéskor.

   tooadd attribútum:

   1. Kattintson a **Hozzáadás attribútum**. Adja meg a hello **neve** és hello válassza hello **érték** hello legördülő menüből.

   2. Kattintson a **mentéséhez.** Új attribútum hello hello táblázatban láthatja.

### <a name="download-hello-azure-ad-metadata-or-certificate"></a>Az Azure AD-metaadatok hello vagy a tanúsítvány letöltése

toodownload hello alkalmazás metaadatainak vagy Azure ad-, tanúsítvány kövesse az alábbi hello lépéseket:

1.  Nyitott hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**

2.  Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.

3.  Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.

4.  Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.

5.  Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.

  * Ha hello alkalmazás jelenik itt meg nem látja, akkor hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját** és set hello **megjelenítése** beállítás túl **Minden alkalmazást.**

6.  Válassza ki az egyszeri bejelentkezés konfigurált hello alkalmazást.

7.  Ha hello alkalmazás betölt, kattintson a hello **egyszeri bejelentkezés** hello alkalmazás bal oldali navigációs menüjében.

8.  Nyissa meg túl**SAML-aláíró tanúsítványa** területen, majd kattintson a **letöltése** oszlop értékét. Attól függően, hogy milyen hello alkalmazásnak szüksége van, az egyszeri bejelentkezés konfigurálása lásd: vagy hello beállítás toodownload hello metaadatainak XML-kódja vagy hello tanúsítványt.

    Az Azure AD egy URL-cím tooget hello metaadatok nem biztosít. hello metaadatok XML-fájlként csak olvasható.

## <a name="how-tooconfigure-federated-single-sign-on-for-a-non-gallery-application"></a>Hogyan tooconfigure összevont egyszeri bejelentkezés egy nem galéria alkalmazáshoz

toohave az Azure AD premium szüksége tooconfigure nem galéria-alkalmazás, és hello alkalmazás támogatja az SAML 2.0-s. Az Azure AD-verziókkal kapcsolatos további információkért látogasson el a [az Azure AD árképzési](https://azure.microsoft.com/pricing/details/active-directory/).

-   [Hello alkalmazás metaadatainak értékek konfigurálása az Azure AD (bejelentkezési URL-cím, az azonosítója, válasz URL-címe)](#configuring-single-sign-on)

-   [Válassza ki a felhasználói azonosítót és a felhasználói attribútumok küldött toobe toohello alkalmazás hozzáadása](#select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application)

-   [Az Azure AD-metaadatok és a tanúsítvány lekérése](#download-the-azure-ad-metadata-or-certificate)

-   [Konfigurálja az Azure AD metaadatok értékeket hello alkalmazásban (bejelentkezési URL-címet, a kibocsátó, a kijelentkezési URL-cím és a tanúsítvány)](#configuring-single-sign-on)

### <a name="configure-hello-applications-metadata-values-in-azure-ad-sign-on-url-identifier-reply-url"></a>Hello alkalmazás metaadatainak értékek konfigurálása az Azure AD (bejelentkezési URL-cím, az azonosítója, válasz URL-címe)

tooconfigure egyszeri bejelentkezés egy alkalmazás, amely nincs az Azure AD-dokumentumtárban hello, kövesse az alábbi hello lépéseket:

1.  Nyitott hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**

2.  Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.

3.  Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.

4.  Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.

5.  Kattintson a hello **hozzáadása** hello jobb felső sarokban lévő hello a gomb **vállalati alkalmazások** panel

6.  Kattintson a **nem-gyűjtemény alkalmazás** a hello **saját alkalmazás felvétele** szakasz

7.  Adja meg hello hello alkalmazás hello **neve** szövegmező.

8.  Kattintson a **Hozzáadás** tooadd hello alkalmazás gomb.

9.  Ha hello alkalmazás betölt, kattintson a hello **egyszeri bejelentkezés** hello alkalmazás bal oldali navigációs menüjében.

10. Válassza ki **SAML-alapú bejelentkezés** a hello **mód** legördülő lista

11. Adja meg a szükséges hello értékeket **tartomány és az URL-címeket.** Ezeket az értékeket kell beszerezni hello alkalmazás gyártójának segítségét.

  1. tooconfigure hello alkalmazásra, amely a kiállító terjesztési hely által kezdeményezett egyszeri Bejelentkezést, adja meg a hello válasz URL-CÍMEN és hello azonosítója.

  2. **Választható lehetőség:** tooconfigure hello alkalmazásra, amely a Szolgáltató által kezdeményezett egyszeri Bejelentkezést, hello bejelentkezési URL-címen egy szükséges érték.

12. A hello **felhasználói attribútumok**, válassza ki a felhasználók a hello egyedi azonosítója hello **felhasználói azonosító** legördülő menüből.

13. **Nem kötelező:** kattintson **megtekintése és szerkesztése a más felhasználói attribútumok** tooedit hello attribútumok küldött toobe toohello alkalmazás hello SAML-jogkivonat felhasználói bejelentkezéskor.

   tooadd attribútum:

   1. Kattintson a **Hozzáadás attribútum**. Adja meg a hello **neve** és hello válassza hello **érték** hello legördülő menüből.

   2. Kattintson a **mentéséhez.** Új attribútum hello hello táblázatban láthatja.

14. Kattintson a **konfigurálása &lt;alkalmazásnév&gt;**  tooaccess dokumentáció tooconfigure egyszeri bejelentkezés hello alkalmazásban. Is, akkor az Azure AD URL-címek és rendelkezik hello alkalmazásához szükséges tanúsítvány.

### <a name="select-user-identifier-and-add-user-attributes-toobe-sent-toohello-application"></a>Válassza ki a felhasználói azonosítót és a felhasználói attribútumok küldött toobe toohello alkalmazás hozzáadása

tooselect hello felhasználói azonosítót, vagy adja hozzá a felhasználói attribútumok, hajtsa végre az alábbi lépéseket hello:

1.  Nyitott hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**

2.  Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.

3.  Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.

4.  Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.

5.  Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.

  * Ha hello alkalmazás jelenik itt meg nem látja, akkor hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját** és set hello **megjelenítése** beállítás túl **Minden alkalmazást.**

6.  Válassza ki az egyszeri bejelentkezés konfigurált hello alkalmazást.

7.  Ha hello alkalmazás betölt, kattintson a hello **egyszeri bejelentkezés** hello alkalmazás bal oldali navigációs menüjében.

8.  A hello **felhasználói attribútumok** szakaszban jelölje be a felhasználók a hello egyedi azonosítója hello **felhasználói azonosító** legördülő menüből. hello kiválasztott beállítás toomatch hello várt értéket kell hello alkalmazás tooauthenticate hello felhasználó.

   >[!NOTE]
   >Az Azure AD válassza hello formátumban hello NameID attribútum (felhasználói azonosító) kijelölt hello érték alapján, vagy a SAML AuthRequest hello hello alkalmazás által kért formátum hello. További információt a Microsoft hello cikk [egyszeri bejelentkezés SAML protokoll](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) az hello rész NameIDPolicy.
   >
   >

9.  tooadd felhasználói attribútumok, kattintson **megtekintése és szerkesztése a más felhasználói attribútumok** tooedit hello attribútumok küldött toobe toohello alkalmazás hello SAML-jogkivonat felhasználói bejelentkezéskor.

   tooadd attribútum:

   1. Kattintson a **Hozzáadás attribútum**. Adja meg a hello **neve** és hello válassza hello **érték** hello legördülő menüből.

   2 kattintson **mentéséhez.** Új attribútum hello hello táblázatban láthatja.

### <a name="download-hello-azure-ad-metadata-or-certificate"></a>Az Azure AD-metaadatok hello vagy a tanúsítvány letöltése

toodownload hello alkalmazás metaadatainak vagy Azure ad-, tanúsítvány kövesse az alábbi hello lépéseket:

1.  Nyitott hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**

2.  Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.

3.  Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.

4.  Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.

5.  Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.

   * Ha hello alkalmazás jelenik itt meg nem látja, akkor hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját** és set hello **megjelenítése** beállítás túl **Minden alkalmazást.**

6.  Válassza ki az egyszeri bejelentkezés konfigurált hello alkalmazást.

7.  Ha hello alkalmazás betölt, kattintson a hello **egyszeri bejelentkezés** hello alkalmazás bal oldali navigációs menüjében.

8.  Nyissa meg túl**SAML-aláíró tanúsítványa** területen, majd kattintson a **letöltése** oszlop értékét. Attól függően, hogy milyen hello alkalmazásnak szüksége van, az egyszeri bejelentkezés konfigurálása lásd: vagy hello beállítás toodownload hello metaadatainak XML-kódja vagy hello tanúsítványt.

    Az Azure AD egy URL-cím tooget hello metaadatok nem biztosít. hello metaadatok XML-fájlként csak olvasható.

## <a name="how-tooconfigure-password-single-sign-on-for-an-azure-ad-gallery-application"></a>Hogyan tooconfigure jelszó egyszeri bejelentkezés az Azure AD gyűjtemény alkalmazáshoz

egy alkalmazás hello Azure AD-galériából tooconfigure kell:

-   [Alkalmazás hozzáadása hello Azure AD-galériából](#add-an-application)

-   [Állítsa be hello alkalmazását jelszó egyszeri bejelentkezést.](#configure-the-application)

### <a name="add-an-application-from-hello-azure-ad-gallery"></a>Alkalmazás hozzáadása hello Azure AD-galériából

az alkalmazás a hello Azure AD-katalógusában, tooadd kövesse hello alábbi lépéseket:

1.  Nyissa meg hello [Azure Portal](https://portal.azure.com) , és jelentkezzen be a egy **globális rendszergazda** vagy **társadminisztrátornak**

2.  Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.

3.  Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.

4.  Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.

5.  Kattintson a hello **hozzáadása** hello jobb felső sarokban lévő hello a gomb **vállalati alkalmazások** panel

6.  A hello **adjon meg egy nevet** hello a szövegmező **hello gyűjteményből Hozzáadás** szakaszban, hello hello alkalmazás neve

7.  Válassza ki a kívánt tooconfigure az egyszeri bejelentkezés hello alkalmazás

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

9.  Felhasználók hozzárendelése toohello alkalmazás.

10. Emellett is megadhatja hello felhasználó nevében hitelesítő adatok hello felhasználók hello sorát kiválasztásával, és kattintson a **frissítéséhez szükséges hitelesítő adatokat** és hello felhasználónév és jelszó megadásával hello felhasználók nevében. Ellenkező esetben a felhasználók fognak felszólító tooenter hello hitelesítő adatok magukat az indítás után.

## <a name="how-tooconfigure-password-single-sign-on-for-a-non-gallery-application"></a>Hogyan tooconfigure jelszó egyszeri bejelentkezés egy nem galéria alkalmazáshoz

egy alkalmazás hello Azure AD-galériából tooconfigure kell:

-   [Nem galéria alkalmazás hozzáadása](#add-a-non-gallery-application)

-   [Állítsa be hello alkalmazását jelszó egyszeri bejelentkezést.](#configure-the-application-for-password-single-sign-on)

### <a name="add-a-non-gallery-application"></a>Nem galéria alkalmazás hozzáadása

az alkalmazás a hello Azure AD-katalógusában, tooadd kövesse hello alábbi lépéseket:

1.  Nyissa meg hello [Azure Portal](https://portal.azure.com) , és jelentkezzen be a egy **globális rendszergazda** vagy **társadminisztrátornak**

2.  Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.

3.  Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.

4.  Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.

5.  Kattintson a hello **hozzáadása** hello jobb felső sarokban lévő hello a gomb **vállalati alkalmazások** panel

6.  Kattintson a **nem-gyűjtemény alkalmazás.**

7.  Adja meg az alkalmazás nevére hello hello **neve** szövegmező. Válassza ki **hozzáadása.**

Egy rövid időszak után kell képes toosee hello alkalmazás konfigurációs panelen.

### <a name="configure-hello-application-for-password-single-sign-on"></a>Állítsa be hello alkalmazását jelszó egyszeri bejelentkezést.

tooconfigure egyszeri bejelentkezés egy alkalmazás kövesse az alábbi hello lépéseket:

1.  Nyitott hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**

2.  Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.

3.  Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.

4.  Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.

5.  Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.

 * Ha hello alkalmazás jelenik itt meg nem látja, akkor hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját** és set hello **megjelenítése** beállítás túl **Minden alkalmazást.**

6.  Válassza ki a kívánt tooconfigure egyszeri bejelentkezés hello alkalmazást.

7.  Ha hello alkalmazás betölt, kattintson a hello **egyszeri bejelentkezés** hello alkalmazás bal oldali navigációs menüjében.

8.  Jelölje be hello mód **jelszóalapú bejelentkezés.**

9.  Adja meg a hello **bejelentkezési URL-cím**. Ez a hello URL-cím, ahol felhasználók adja meg a felhasználónevet és jelszót toosign a számára. Győződjön meg arról, mezők hello bejelentkezés láthatók hello URL-címen.

10. Felhasználók hozzárendelése toohello alkalmazás.

11. Emellett is megadhatja hello felhasználó nevében hitelesítő adatok hello felhasználók hello sorát kiválasztásával, és kattintson a **frissítéséhez szükséges hitelesítő adatokat** és hello felhasználónév és jelszó megadásával hello felhasználók nevében. Ellenkező esetben a felhasználók fognak felszólító tooenter hello hitelesítő adatok magukat az indítás után.

## <a name="how-tooassign-a-user-tooan-application-directly"></a>Hogyan tooassign közvetlenül egy felhasználói tooan alkalmazást

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

## <a name="if-these-troubleshooting-steps-do-not-hello-resolve-hello-issue"></a>Ha a lépések nem hello hello probléma megoldásához.

Nyisson meg egy támogatási jegy hello ha rendelkezésre áll a következő információkat:

-   Megfelelési hiba azonosítója

-   Egyszerű felhasználónév (felhasználó e-mail címe)

-   A TenantID

-   Böngésző típusa

-   Időzóna és idő/időkeretre során hiba történik.

-   Fiddler nyomkövetések

## <a name="next-steps"></a>Következő lépések
[Adja meg az egyszeri bejelentkezés tooyour alkalmazások alkalmazásproxyval](active-directory-application-proxy-sso-using-kcd.md)

