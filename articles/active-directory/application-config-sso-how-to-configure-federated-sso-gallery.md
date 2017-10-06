---
title: "egyszeri bejelentkezés az Azure AD-katalógusában alkalmazás összevont aaaHow tooconfigure |} Microsoft Docs"
description: "Hogyan tooconfigure összevont egyszeri bejelentkezéshez egy meglévő Azure AD-katalógusában és használati oktatóanyagok tooget is gyorsan az"
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
ms.openlocfilehash: a93de021fddd253e4fe663c221b822d12625fd54
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-federated-single-sign-on-for-an-azure-ad-gallery-application"></a>Hogyan tooconfigure összevont egyszeri bejelentkezés az Azure AD-katalógusában alkalmazáshoz

Összes alkalmazás engedélyezve van a vállalati egyszeri bejelentkezési képesség hello Azure AD-katalógus elérhető egy részletes oktatóanyag rendelkezik. Van-e hozzáférési hello [kapcsolatos bemutatók felsorolása toointegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/) részletes, lépésenkénti útmutatást.

## <a name="overview-of-steps-required"></a>A szükséges lépések – áttekintés
egy alkalmazás hello Azure AD-galériából tooconfigure kell:

-   [Alkalmazás hozzáadása hello Azure AD-galériából](#add-an-application-from-the-azure-ad-gallery)

-   [Hello alkalmazás metaadatainak értékek konfigurálása az Azure AD (bejelentkezési URL-cím, az azonosítója, válasz URL-címe)](#configure-single-sign-on-for-an-application-from-the-azure-ad-gallery)

-   [Válassza ki a felhasználói azonosítót és a felhasználói attribútumok küldött toobe toohello alkalmazás hozzáadása](#select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application)

-   [Az Azure AD-metaadatok és a tanúsítvány lekérése](#download-the-azure-ad-metadata-or-certificate)

-   [Konfigurálja az Azure AD metaadatok értékeket hello alkalmazásban (bejelentkezési URL-címet, a kibocsátó, a kijelentkezési URL-cím és a tanúsítvány)](#configure-single-sign-on-for-an-application-from-the-azure-ad-gallery)

-   [Felhasználók hozzárendelése toohello alkalmazás](#assign-users-to-the-application)

## <a name="add-an-application-from-hello-azure-ad-gallery"></a>Alkalmazás hozzáadása hello Azure AD-galériából

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

Rövid időn belül kell tudni toosee hello alkalmazás konfigurációs panelen.

## <a name="configure-single-sign-on-for-an-application-from-hello-azure-ad-gallery"></a>Egyszeri bejelentkezés egy alkalmazás hello Azure AD-galériából konfigurálása

tooconfigure egyszeri bejelentkezés egy alkalmazás kövesse az alábbi hello lépéseket:

1.  Megnyitás hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **társadminisztrátor**.

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

   1. Kattintson a **mentéséhez.** Új attribútum hello hello táblázatban láthatja.

13. Kattintson a **konfigurálása &lt;alkalmazásnév&gt;**  tooaccess dokumentáció tooconfigure egyszeri bejelentkezés hello alkalmazásban. Is hogy rendelkezik hello metadata URL-címek és a szükséges tanúsítványok toosetup SSO hello alkalmazással.

14. Kattintson a **mentése** toosave hello konfigurációs.

15. Felhasználók hozzárendelése toohello alkalmazás.

## <a name="select-user-identifier-and-add-user-attributes-toobe-sent-toohello-application"></a>Válassza ki a felhasználói azonosítót és a felhasználói attribútumok küldött toobe toohello alkalmazás hozzáadása

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

   2. Kattintson a **Save** (Mentés) gombra. Új attribútum hello hello táblázatban láthatja.

## <a name="download-hello-azure-ad-metadata-or-certificate"></a>Az Azure AD-metaadatok hello vagy a tanúsítvány letöltése

toodownload hello alkalmazás metaadatainak vagy Azure ad-, tanúsítvány kövesse az alábbi hello lépéseket:

1.  Nyitott hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**

2.  Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.

3.  Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.

4.  Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.

5.  Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.

  *  Ha hello alkalmazás jelenik itt meg nem látja, akkor hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját** és set hello **megjelenítése** beállítás túl **Minden alkalmazás**.

6.  Válassza ki az egyszeri bejelentkezés konfigurált hello alkalmazást.

7.  Ha hello alkalmazás betölt, kattintson a hello **egyszeri bejelentkezés** hello alkalmazás bal oldali navigációs menüjében.

8.  Nyissa meg túl**SAML-aláíró tanúsítványa** területen, majd kattintson a **letöltése** oszlop értékét. Attól függően, hogy milyen hello alkalmazásnak szüksége van, az egyszeri bejelentkezés konfigurálása lásd: vagy hello beállítás toodownload hello metaadatainak XML-kódja vagy hello tanúsítványt.

Az Azure AD egy URL-cím tooget hello metaadatok nem biztosít. hello metaadatok XML-fájlként csak olvasható.

## <a name="assign-users-toohello-application"></a>Felhasználók hozzárendelése toohello alkalmazás

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

Rövid időn belül a kijelölt hello felhasználók kell ezeket az alkalmazásokat használó hello hello megoldás leírása szakaszban ismertetett módszerekkel tudja toolaunch.

## <a name="customizing-hello-saml-claims-sent-tooan-application"></a>Hello SAML jogcímek testreszabása küldött tooan alkalmazás

toolearn hogyan toocustomize hello SAML attribútum típusú jogcímek küldött tooyour alkalmazás, lásd: [hozzárendelése az Azure Active Directory-jogcímek](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) további információt.

## <a name="next-steps"></a>Következő lépések
[Adja meg az egyszeri bejelentkezés tooyour alkalmazások alkalmazásproxyval](active-directory-application-proxy-sso-using-kcd.md)



