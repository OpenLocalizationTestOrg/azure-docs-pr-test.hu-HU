---
title: "hozzárendelt aaaAn alkalmazás nem jelenik meg a hozzáférési panel hello |} Microsoft Docs"
description: "Ezért az alkalmazás nem jelenik meg a hozzáférési Panel hello hibaelhárítása"
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
ms.openlocfilehash: 089883f406267df4552c7fc991883f335ad49fd5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="an-assigned-application-is-not-appearing-on-hello-access-panel"></a>Kijelölt alkalmazást nem szereplő hello hozzáférési panel

hello hozzáférési Panel egy webes portál, amely lehetővé teszi a felhasználó munkahelyi vagy iskolai fiókkal az Azure Active Directory (Azure AD) tooview és kezdő felhőalapú alkalmazások, hogy hello Azure AD-rendszergazda rendelkezik hozzáféréssel őket. Ezeket az alkalmazásokat úgy vannak konfigurálva, hello Azure AD portálon hello felhasználó nevében. hello alkalmazás megfelelően kell konfigurálni, és a hozzárendelt toohello felhasználó vagy csoport hello felhasználó tagja a hozzáférési Panel hello toosee hello alkalmazás.

a következő kategóriák hello esik azért jelent meg a felhasználó alkalmazások hello típusa:

-   Office 365-alkalmazások

-   Összevonási-alapú egyszeri bejelentkezési modellel konfigurált a Microsoft és külső alkalmazások

-   Jelszó-alapú egyszeri bejelentkezés alkalmazásokhoz

-   A már meglévő SSO megoldásaival alkalmazások

## <a name="general-issues-toocheck-first"></a>Általános problémák első toocheck

-   Ha egy alkalmazás előbbiekben felvett tooa felhasználói, próbálja toosign bejövő és kimenő adatforgalma újra hello felhasználói hozzáférési panelre után néhány perc toosee Ha hello adja hozzá.

-   A licenc csak el lett távolítva a felhasználó vagy csoport hello felhasználó esetén tagja ennek attól függően, hogy hello méretét és összetettségét hello csoportot a végzett módosítások toobe hosszú ideig is eltarthat. Lehetővé teszi további alkalommal jelentkezik be a hozzáférési Panel hello előtt.

## <a name="problems-related-tooapplication-configuration"></a>Kapcsolódó tooapplication konfigurációs problémák

Egy alkalmazás nem lehetséges, hogy megjelenjen a a felhasználó hozzáférési Panel, mert nincs megfelelően konfigurálva. Az alábbiakban néhány módszert tooapplication kapcsolódó konfigurációs problémák hibaelhárítását hajthatja végre:

-   [Hogyan tooconfigure összevont egyszeri bejelentkezés az Azure AD gyűjtemény alkalmazáshoz](#how-to-configure-federated-single-sign-on-for-an-azure-ad-gallery-application)

-   [Hogyan tooconfigure összevont egyszeri bejelentkezés egy nem galéria alkalmazáshoz](#how-to-configure-federated-single-sign-on-for-a-non-gallery-application)

-   [Hogyan tooconfigure jelszó egyszeri bejelentkezést egy Azure AD-gyűjtemény alkalmazást alkalmazás](#how-to-configure-password-single-sign-on-for-a-non-gallery-application)

-   [Hogyan tooconfigure jelszó egyszeri bejelentkezés alkalmazás nem galéria alkalmazáshoz](#how-to-configure-password-single-sign-on-for-a-non-gallery-application)

### <a name="how-tooconfigure-federated-single-sign-on-for-an-azure-ad-gallery-application"></a>Hogyan tooconfigure összevont egyszeri bejelentkezés az Azure AD gyűjtemény alkalmazáshoz

Összes alkalmazás hello Azure AD-dokumentumtárban vállalati egyszeri bejelentkezésre alkalmas engedélyezve van elérhető részletes oktatóanyaga. Van-e hozzáférési hello [kapcsolatos bemutatók felsorolása toointegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/) kapcsolatos részletek részletes útmutatás.

egy alkalmazás hello Azure AD-galériából tooconfigure kell:

-   [Alkalmazás hozzáadása hello Azure AD-galériából](#add-an-application-from-the-azure-ad-gallery)

-   [Hello alkalmazás metaadatainak értékek konfigurálása az Azure AD (bejelentkezési URL-cím, az azonosítója, válasz URL-címe)](#configure-single-sign-on-for-an-application-from-the-azure-ad-gallery)

-   [Válassza ki a felhasználói azonosítót és a felhasználói attribútumok küldött toobe toohello alkalmazás hozzáadása](#select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application)

-   [Az Azure AD-metaadatok és a tanúsítvány lekérése](#download-the-azure-ad-metadata-or-certificate)

-   [Konfigurálja az Azure AD metaadatok értékeket hello alkalmazásban (bejelentkezési URL-címet, a kibocsátó, a kijelentkezési URL-cím és a tanúsítvány)](#configure-single-sign-on-for-an-application-from-the-azure-ad-gallery)

#### <a name="add-an-application-from-hello-azure-ad-gallery"></a>Alkalmazás hozzáadása hello Azure AD-galériából

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

#### <a name="configure-single-sign-on-for-an-application-from-hello-azure-ad-gallery"></a>Egyszeri bejelentkezés egy alkalmazás hello Azure AD-galériából konfigurálása

tooconfigure egyszeri bejelentkezés egy alkalmazás kövesse az alábbi hello lépéseket:

1.  Nyitott hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**

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

#### <a name="select-user-identifier-and-add-user-attributes-toobe-sent-toohello-application"></a>Válassza ki a felhasználói azonosítót és a felhasználói attribútumok küldött toobe toohello alkalmazás hozzáadása

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

   2. Kattintson a **mentéséhez.** Új attribútum hello hello tábla jelenik meg.

#### <a name="download-hello-azure-ad-metadata-or-certificate"></a>Az Azure AD-metaadatok hello vagy a tanúsítvány letöltése

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

### <a name="how-tooconfigure-federated-single-sign-on-for-a-non-gallery-application"></a>Hogyan tooconfigure összevont egyszeri bejelentkezés egy nem galéria alkalmazáshoz

toohave az Azure AD premium szüksége tooconfigure nem galéria-alkalmazás, és hello alkalmazás támogatja az SAML 2.0-s. Az Azure AD-verziókkal kapcsolatos további információkért látogasson el a [az Azure AD árképzési](https://azure.microsoft.com/pricing/details/active-directory/).

-   [Hello alkalmazás metaadatainak értékek konfigurálása az Azure AD (bejelentkezési URL-cím, az azonosítója, válasz URL-címe)](#configuring-single-sign-on)

-   [Válassza ki a felhasználói azonosítót és a felhasználói attribútumok küldött toobe toohello alkalmazás hozzáadása](#select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application)

-   [Az Azure AD-metaadatok és a tanúsítvány lekérése](#download-the-azure-ad-metadata-or-certificate)

-   [Konfigurálja az Azure AD metaadatok értékeket hello alkalmazásban (bejelentkezési URL-címet, a kibocsátó, a kijelentkezési URL-cím és a tanúsítvány)](#configuring-single-sign-on)

#### <a name="configure-hello-applications-metadata-values-in-azure-ad-sign-on-url-identifier-reply-url"></a>Hello alkalmazás metaadatainak értékek konfigurálása az Azure AD (bejelentkezési URL-cím, az azonosítója, válasz URL-címe)

tooconfigure egyszeri bejelentkezés egy alkalmazás, amely nincs az Azure AD-dokumentumtárban hello, kövesse az alábbi hello lépéseket:

1.  Nyitott hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**

2.  Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.

3.  Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.

4.  Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.

5.  Kattintson a hello **hozzáadása** hello jobb felső sarokban lévő hello a gomb **vállalati alkalmazások** panelen.

6.  Kattintson a **nem-gyűjtemény alkalmazás** a hello **saját alkalmazás felvétele** szakasz.

7.  Adja meg hello hello alkalmazás hello **neve** szövegmező.

8.  Kattintson a **Hozzáadás** tooadd hello alkalmazás gomb.

9.  Ha hello alkalmazás betölt, kattintson a hello **egyszeri bejelentkezés** hello alkalmazás bal oldali navigációs menüjében.

10. Válassza ki **SAML-alapú bejelentkezés** a hello **mód** legördülő menüből.

11. Adja meg a szükséges hello értékeket **tartomány és az URL-címeket.** Ezeket az értékeket kell beszerezni hello alkalmazás gyártójának segítségét.

   1. tooconfigure hello alkalmazásra, amely a kiállító terjesztési hely által kezdeményezett egyszeri Bejelentkezést, adja meg a hello válasz URL-CÍMEN és hello azonosítója.

   2.  **Választható lehetőség:** tooconfigure hello alkalmazásra, amely a Szolgáltató által kezdeményezett egyszeri Bejelentkezést, hello bejelentkezési URL-címen egy szükséges érték.

12. A hello **felhasználói attribútumok**, válassza ki a felhasználók a hello egyedi azonosítója hello **felhasználói azonosító** legördülő menüből.

13. **Nem kötelező:** kattintson **megtekintése és szerkesztése a más felhasználói attribútumok** tooedit hello attribútumok küldött toobe toohello alkalmazás hello SAML-jogkivonat felhasználói bejelentkezéskor.

   tooadd attribútum:

   1. Kattintson a **Hozzáadás attribútum**. Adja meg a hello **neve** és hello válassza hello **érték** hello legördülő menüből.

   2. Kattintson a **mentéséhez.** Új attribútum hello hello táblázatban láthatja.

14. Kattintson a **konfigurálása &lt;alkalmazásnév&gt;**  tooaccess dokumentáció tooconfigure egyszeri bejelentkezés hello alkalmazásban. Is, akkor az Azure AD URL-címek és rendelkezik hello alkalmazásához szükséges tanúsítvány.

#### <a name="select-user-identifier-and-add-user-attributes-toobe-sent-toohello-application"></a>Válassza ki a felhasználói azonosítót és a felhasználói attribútumok küldött toobe toohello alkalmazás hozzáadása

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

   2. Kattintson a **mentéséhez.** Új attribútum hello hello táblázatban láthatja.

#### <a name="download-hello-azure-ad-metadata-or-certificate"></a>Az Azure AD-metaadatok hello vagy a tanúsítvány letöltése

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

### <a name="how-tooconfigure-password-single-sign-on-for-an-azure-ad-gallery-application"></a>Hogyan tooconfigure jelszó egyszeri bejelentkezés az Azure AD gyűjtemény alkalmazáshoz

egy alkalmazás hello Azure AD-galériából tooconfigure kell:

-   [Alkalmazás hozzáadása hello Azure AD-galériából](#add-an-application-from-the-azure-ad-gallery)

-   [Állítsa be hello alkalmazását jelszó egyszeri bejelentkezést.](#configure-the-application-for-password-single-sign-on)

#### <a name="add-an-application-from-hello-azure-ad-gallery"></a>Alkalmazás hozzáadása hello Azure AD-galériából

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

#### <a name="configure-hello-application-for-password-single-sign-on"></a>Állítsa be hello alkalmazását jelszó egyszeri bejelentkezést.

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

9.  [Felhasználók hozzárendelése toohello alkalmazás](#how-to-assign-a-user-to-an-application-directly).

10. Emellett is megadhatja hello felhasználó nevében hitelesítő adatok hello felhasználók hello sorát kiválasztásával, és kattintson a **frissítéséhez szükséges hitelesítő adatokat** és hello felhasználónév és jelszó megadásával hello felhasználók nevében. Ellenkező esetben a felhasználók fognak felszólító tooenter hello hitelesítő adatok magukat az indítás után.

### <a name="how-tooconfigure-password-single-sign-on-for-a-non-gallery-application"></a>Hogyan tooconfigure jelszó egyszeri bejelentkezés egy nem galéria alkalmazáshoz

egy alkalmazás hello Azure AD-galériából tooconfigure kell:

-   [Nem galéria alkalmazás hozzáadása](#add-a-non-gallery-application)

-   [Állítsa be hello alkalmazását jelszó egyszeri bejelentkezést.](#configure-the-application-for-password-single-sign-on)

#### <a name="add-a-non-gallery-application"></a>Nem galéria alkalmazás hozzáadása

az alkalmazás a hello Azure AD-katalógusában, tooadd kövesse hello alábbi lépéseket:

1.  Megnyitás hello [Azure Portal](https://portal.azure.com) , és jelentkezzen be egy **globális rendszergazda** vagy **társadminisztrátor**.

2.  Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.

3.  Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.

4.  Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.

5.  Kattintson a hello **hozzáadása** hello jobb felső sarokban lévő hello a gomb **vállalati alkalmazások** panelen.

6.  Kattintson a **nem-gyűjtemény alkalmazás.**

7.  Adja meg az alkalmazás nevére hello hello **neve** szövegmező. Válassza ki **hozzáadása.**

Egy rövid időszak után kell képes toosee hello alkalmazás konfigurációs panelen.

#### <a name="configure-hello-application-for-password-single-sign-on"></a>Állítsa be hello alkalmazását jelszó egyszeri bejelentkezést.

tooconfigure egyszeri bejelentkezés egy alkalmazás kövesse az alábbi hello lépéseket:

1.  Nyitott hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**

2.  Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.

3.  Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.

4.  Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.

5.  Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.

    1.  Ha hello alkalmazás jelenik itt meg nem látja, akkor hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját** és set hello **megjelenítése** beállítás túl **Minden alkalmazást.**

6.  Válassza ki a kívánt tooconfigure egyszeri bejelentkezés hello alkalmazást.

7.  Ha hello alkalmazás betölt, kattintson a hello **egyszeri bejelentkezés** hello alkalmazás bal oldali navigációs menüjében.

8.  Jelölje be hello mód **jelszóalapú bejelentkezés.**

9.  Adja meg a hello **bejelentkezési URL-cím**. Ez a hello URL-cím, ahol felhasználók adja meg a felhasználónevet és jelszót toosign a számára. Győződjön meg arról, mezők hello bejelentkezés láthatók hello URL-címen.

10. [Felhasználók hozzárendelése toohello alkalmazás](#how-to-assign-a-user-to-an-application-directly).

11. Emellett is megadhatja hello felhasználó nevében hitelesítő adatok hello felhasználók hello sorát kiválasztásával, és kattintson a **frissítéséhez szükséges hitelesítő adatokat** és hello felhasználónév és jelszó megadásával hello felhasználók nevében. Ellenkező esetben a felhasználók fognak felszólító tooenter hello hitelesítő adatok magukat az indítás után.

## <a name="problems-related-tooassigning-applications-toousers"></a>Problémák kapcsolódó tooassigning alkalmazások toousers

A felhasználó nem lehet annak, hogy egy alkalmazás a hozzáférési panelen, mert ezek nincsenek hozzárendelve toohello alkalmazás. Az alábbiakban néhány módon toocheck:

-   [Ellenőrizze, hogy ha a felhasználó hozzá van rendelve toohello alkalmazás](#check-if-a-user-is-assigned-to-the-application)

-   [Hogyan tooassign közvetlenül egy felhasználói tooan alkalmazást](#how-to-assign-a-user-to-an-application-directly)

-   [Ellenőrizze, hogy a felhasználó hozzá van rendelve a tooa licenc kapcsolódó toohello alkalmazás](#check-if-a-user-is-under-a-license-related-to-the-application)

-   [Hogyan tooassign licenc tooa felhasználó](#how-to-assign-a-user-a-license)

### <a name="check-if-a-user-is-assigned-toohello-application"></a>Ellenőrizze, hogy ha a felhasználó hozzá van rendelve toohello alkalmazás

toocheck a felhasználó hozzá van rendelve a toohello alkalmazást, kövesse az hello alábbi lépéseket:

1.  Nyissa meg hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be a egy **globális rendszergazdája.**

2.  Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.

3.  Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.

4.  Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.

5.  Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.

6.  **Keresési** a szóban forgó hello alkalmazás hello nevét.

7.  Kattintson a **felhasználók és csoportok**.

8.  Ellenőrizze a toosee, ha a felhasználó a toohello alkalmazás hozzá van rendelve.

   * Ha nem hello kövesse "hogyan tooassign közvetlenül a felhasználói tooan alkalmazás" toodo így.

### <a name="how-tooassign-a-user-tooan-application-directly"></a>Hogyan tooassign közvetlenül egy felhasználói tooan alkalmazást

tooassign felhasználók tooan egy vagy több alkalmazás közvetlenül, kövesse az alábbi hello lépéseket:

1.  Megnyitás hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda**.

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

### <a name="check-if-a-user-is-under-a-license-related-toohello-application"></a>Annak ellenőrzése, hogy egy felhasználó egy licenc kapcsolódó toohello alkalmazás

toocheck a felhasználó hozzárendelt licencek, hajtsa végre hello alábbi lépéseket:

1.  Nyissa meg hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be a egy **globális rendszergazdája.**

2.  Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.

3.  Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.

4.  Kattintson a **felhasználók és csoportok** hello navigációs menüjében.

5.  Kattintson a **minden felhasználó**.

6.  **Keresési** hello felhasználó érdekli, és **hello sorban kattintson** tooselect.

7.  Kattintson a **licencek** toosee licencek hello felhasználóhoz van rendelve.

  * Hello felhasználó van hozzárendelve, tooan Office-licencet az első fél Office alkalmazások tooappear ezzel engedélyezi a felhasználó hozzáférési Panel hello.

### <a name="how-tooassign-a-user-a-license"></a>Hogyan tooassign egy felhasználói licenc 

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

## <a name="problems-related-tooassigning-applications-toogroups"></a>Problémák kapcsolódó tooassigning alkalmazások toogroups

Egy felhasználó lehet annak, hogy egy alkalmazás a hozzáférési Panel a mert hello alkalmazás rendelt csoportba. Az alábbiakban néhány módon toocheck:

-   [A felhasználói csoporttagság ellenőrzése](#check-a-users-group-memberships)

-   [Hogyan tooassign egy alkalmazás tooa csoportosítani közvetlenül](#how-to-assign-an-application-to-a-group-directly)

-   [Ellenőrizze, hogy a felhasználó csoportba tooa licenc hozzárendelése](#check-if-a-user-is-part-of-group-assigned-to-a-license)

-   [Hogyan tooassign tooa licenccsoportok](#how-to-assign-a-license-to-a-group)

### <a name="check-a-users-group-memberships"></a>A felhasználói csoporttagság ellenőrzése

toocheck egy csoport tagságát, hajtsa végre hello alábbi lépéseket:

1.  Nyissa meg hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be a egy **globális rendszergazdája.**

2.  Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.

3.  Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.

4.  Kattintson a **felhasználók és csoportok** hello navigációs menüjében.

5.  Kattintson a **minden felhasználó**.

6.  **Keresési** hello felhasználó érdekli, és **hello sorban kattintson** tooselect.

7.  Kattintson a **csoportok**.

8.  Toosee ellenőrizze, hogy a felhasználók egy csoportjának toohello alkalmazás részét.

  * Ha azt szeretné, hogy tooremove hello felhasználói hello csoportból **hello sorban kattintson** hello csoport, és válassza a törlés.

### <a name="how-tooassign-an-application-tooa-group-directly"></a>Hogyan tooassign egy alkalmazás tooa csoportosítani közvetlenül

egy vagy több tooassign csoportok közvetlenül, tooan alkalmazás kövesse hello alábbi lépéseket:

1.  Megnyitás hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda**.

2.  Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.

3.  Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.

4.  Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.

5.  Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.

  * Ha hello alkalmazás jelenik itt meg nem látja, akkor hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját** és set hello **megjelenítése** beállítás túl **Minden alkalmazást.**

6.  Válassza ki a kívánt felhasználó toofrom hello listáját tooassign hello alkalmazást.

7.  Amikor hello alkalmazás betölt, kattintson a **felhasználók és csoportok** hello alkalmazás bal oldali navigációs menüjében.

8.  Hello kattintson **Hozzáadás** gomb felett hello **felhasználók és csoportok** lista tooopen hello **hozzáadása hozzárendelés** panelen.

9.  hello kattintson **felhasználók és csoportok** hello a választó **hozzáadása hozzárendelés** panelen.

10. Hello típusa **teljes csoportnév** érdekli hello való hozzárendelése hello csoport **Keresés név vagy e-mail cím alapján** keresőmezőbe.

11. Hello rámutat **csoport** a hello lista tooreveal egy **jelölőnégyzet**. Kattintson a profil fénykép vagy embléma tooadd hello jelölőnégyzet következő toohello csoport a felhasználó toohello **kijelölt** listája.

12. **Nem kötelező:** Ha túl szeretné**egynél több csoport hozzáadása**, egy másik típus **teljes csoport neve** hello be **Keresés név vagy e-mail cím alapján** keresőmezőbe hello jelölőnégyzet tooadd kattintson a csoport toohello **kijelölt** listája.

13. Ha befejezte a csoportok kiválasztásával, kattintson a hello **válasszon** gomb tooadd őket felhasználók és csoportok toobe toohello listája toohello alkalmazás hozzárendelése.

14. **Választható lehetőség:** hello kattintson **Szerepkörválasztás** hello a választó **hozzáadása hozzárendelés** panel tooselect egy szerepkör tooassign toohello csoportokat kijelölt.

15. Kattintson a hello **hozzárendelése** gomb tooassign hello alkalmazás toohello kijelölt csoportok.

Miután rövid időn belül, kijelölt hello felhasználók kell tudni toolaunch ezeket az alkalmazásokat hello a hozzáférési Panel.

### <a name="check-if-a-user-is-part-of-group-assigned-tooa-license"></a>Ellenőrizze, hogy a felhasználó csoportba tooa licenc hozzárendelése

1.  Nyissa meg hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be a egy **globális rendszergazdája.**

2.  Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.

3.  Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.

4.  Kattintson a **felhasználók és csoportok** hello navigációs menüjében.

5.  Kattintson a **minden felhasználó**.

6.  **Keresési** hello felhasználó érdekli, és **hello sorban kattintson** tooselect.

7.  Kattintson a **csoportok**.

8.  Kattintson egy meghatározott csoport hello sorát.

9.  Kattintson a **licencek** melyik licencek hello csoporthoz rendelt tooit toosee.

   * Ha hello csoport hozzárendelt tooan Office-licencet ezzel előfordulhat, hogy engedélyezi bizonyos első fél Office alkalmazások tooappear hello felhasználói hozzáférési panelen.

### <a name="how-tooassign-a-license-tooa-group"></a>Hogyan tooassign tooa licenccsoportok

tooassign tooa licenccsoport, kövesse a hello alábbi lépéseket:

1.  Nyissa meg hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be a egy **globális rendszergazdája.**

2.  Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.

3.  Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.

4.  Kattintson a **felhasználók és csoportok** hello navigációs menüjében.

5.  Kattintson a **összes csoport**.

6.  **Keresési** hello csoport érdekli, és **hello sorban kattintson** tooselect.

7.  Kattintson a **licencek** toosee mely licencek hello csoport jelenleg hozzá van rendelve.

8.  Kattintson a hello **hozzárendelése** gombra.

9.  Válassza ki **egy vagy több termék** választható termékek hello listája.

10. **Nem kötelező** hello kattintson **hozzárendelés beállításai** elem toogranularly rendelje hozzá a termékek. Kattintson a **Ok** amikor ez befejeződik.

11. Kattintson a hello **hozzárendelése** tooassign licencek toothis csoportbiztonsági gombra. Ez eltarthat egy ideig, attól függően, hogy hello méretét és összetettségét hello csoport.

>[!NOTE]
>toodo Ez gyorsabb, érdemes lehet ideiglenesen hozzárendelés közvetlenül a licenc toohello felhasználó. 
>
>

## <a name="next-steps"></a>Következő lépések
[Adja hozzá az új felhasználók tooAzure Active Directory](active-directory-users-create-azure-portal.md)

