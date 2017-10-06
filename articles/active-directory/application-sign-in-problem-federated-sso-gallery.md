---
title: "tooa gyűjtemény alkalmazás konfigurált összevont bejelentkezés aaaProblems egyszeri bejelentkezés |} Microsoft Docs"
description: "Útmutató a hello bizonyos hibákat, amikor bejelentkezik egy alkalmazás már konfigurálta az SAML-alapú összevont egyszeri bejelentkezés az Azure ad-vel"
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
ms.openlocfilehash: ba20a4904860cf063967029cad33fb80f16e4956
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="problems-signing-in-tooa-gallery-application-configured-for-federated-single-sign-on"></a>Összevont egyszeri bejelentkezés beállítása tooa gyűjtemény alkalmazásban problémák

tootroubleshoot a probléma tooverify hello alkalmazás konfigurációja az alábbiak szerint Azure AD van szüksége:

-   Az Azure AD-gyűjtemény alkalmazást hello összes hello konfigurációs lépéseket követte.

-   hello azonosító és az aad-ben megadott válasz URL-cím egyezik azok hello alkalmazás a várt értékek

-   Hozzárendelt felhasználók toohello alkalmazás

## <a name="application-not-found-in-directory"></a>Az alkalmazás nem található a könyvtárban

*Hiba AADSTS70001: "Https://contoso.com" azonosítójú alkalmazás nem található hello directory*.

**Lehetséges ok**

hello kibocsátó attribútum küldi hello alkalmazás tooAzure a hello SAML-kérelmet AD nem felel meg az Azure AD hello alkalmazásban konfigurált hello azonosító értéket.

**Megoldás**

Ellenőrizze, hogy azt az egyező hello Azure AD-ben konfigurált azonosító értéket hello SAML-kérelmet található hello kibocsátó attribútum:

1.  Nyitott hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**

2.  Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.

3.  Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.

4.  Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.

5.  Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.

  * Ha hello alkalmazás jelenik itt meg nem látja, akkor hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját** és set hello **megjelenítése** beállítás túl **Minden alkalmazást.**

6.  Válassza ki a kívánt tooconfigure egyszeri bejelentkezés hello alkalmazás

7.  Ha hello alkalmazás betölt, kattintson a hello **egyszeri bejelentkezés** hello alkalmazás bal oldali navigációs menüjében.

8.  Nyissa meg túl**tartomány és az URL-címek** szakasz. Ellenőrizze a szövegmező van megfelelő hello hello azonosító értéket hello hiba jelenik meg a következő hello hello értéket.

Után hello azonosító értéket frissítette az Azure ad-ben, és megfelelő hello értéke hello alkalmazás a hello SAML-kérelmet küld, meg kell tudni toosign toohello alkalmazásban.

## <a name="hello-reply-address-does-not-match-hello-reply-addresses-configured-for-hello-application"></a>hello a válaszcím nem egyezik meg a hello válasz címek hello alkalmazáshoz konfigurált.

*AADSTS50011. hiba: a hello válaszcímet "https://contoso.com" nem felel meg a hello válasz címek hello alkalmazáshoz konfigurált*

**Lehetséges ok**

hello hello SAML kérelem AssertionConsumerServiceURL értéke nem egyezik a hello válasz URL-CÍMEN értékkel vagy mintával konfigurálva az Azure ad-ben. hello hello SAML-kérelmet AssertionConsumerServiceURL értéket az hello hiba látható hello URL-cím.

**Megoldás**

Győződjön meg arról, hogy hello AssertionConsumerServiceURL értéket hello SAML-kérelmet az egyező hello válasz URL-címével konfigurálva az Azure AD.

1.  Nyitott hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**

2.  Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.

3.  Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.

4.  Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.

5.  Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.

  * Ha hello alkalmazás jelenik itt meg nem látja, akkor hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját** és set hello **megjelenítése** beállítás túl **Minden alkalmazást.**

6.  Válassza ki a kívánt tooconfigure egyszeri bejelentkezés hello alkalmazás

7.  Ha hello alkalmazás betölt, kattintson a hello **egyszeri bejelentkezés** hello alkalmazás bal oldali navigációs menüjében.

8.  Nyissa meg túl**tartomány és az URL-címek** szakasz. Győződjön meg arról, vagy frissítse hello válasz URL-CÍMEN szövegmező toomatch hello AssertionConsumerServiceURL érték az SAML-kérelmet hello hello értéket.  
    * Ha nem lát hello válasz URL-cím beviteli mező, válassza ki a hello **megjelenítése speciális URL-beállításainak** jelölőnégyzetet.

Után az Azure ad-ben frissítette hello válasz URL-címével, és ennek megfelelő hello érték küld hello alkalmazás hello a SAML-kérelmet, toohello alkalmazás képes toosign kell lennie.

## <a name="user-not-assigned-a-role"></a>Nem hozzárendelt felhasználó

*Hiba AADSTS50105: hello bejelentkezett felhasználó "brian@contoso.com" nem szerepét tooa hello alkalmazás*.

**Lehetséges ok**

hello felhasználó nem rendelkezik hozzáféréssel toohello alkalmazás az Azure ad-ben.

**Megoldás**

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

## <a name="not-a-valid-saml-request"></a>Nem egy érvényes SAML-kérés

*Hiba AADSTS75005: hello kérelme, mert nem egy Saml2 protokoll érvényes üzenetet.*

**Lehetséges ok**

Az Azure AD nem támogatja az SAML-kérelmek az egyszeri bejelentkezés hello alkalmazás által küldött hello. Néhány gyakori kérdések a következők:

-   Hiányzik a kötelező mezőket hello SAML-kérelmet a

-   SAML kódolt kérelmek módja

**Megoldás**

1.  Rögzítse a SAML-kérelmet. hello az oktatóanyagot követve [hogyan toodebug SAML-alapú egyszeri bejelentkezés tooapplications az Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-debugging) toolearn hogyan toocapture hello SAML kérelem.

2.  Forduljon a hello alkalmazás gyártójának és megosztásnévhez:

   -   SAML-kérelmet

   -   [Azure AD-egyszeri bejelentkezés SAML protokoll követelmények](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference)

Ellenőrizni kell az egyszeri bejelentkezés hello Azure AD SAML-alapú megvalósítás támogatják.

## <a name="no-resource-in-requiredresourceaccess-list"></a>Nincs erőforrás requiredResourceAccess listában

*Hiba AADSTS65005:hello ügyfélalkalmazás kért hozzáférés tooresource "00000002-0000-0000-c000-000000000000'. A kérelem sikertelen volt, mert hello ügyfél ehhez az erőforráshoz nem megadott requiredResourceAccess listáján található*.

**Lehetséges ok**

hello alkalmazásobjektum sérült.

**Megoldás: 1. lehetőséget**

toosolve hello problémát, vegye fel a hello egyedi azonosító értéket hello Azure AD-konfigurációban. tooadd azonosító értéket, kövesse az alábbi hello lépéseket:

1.  Nyitott hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**

2.  Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.

3.  Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.

4.  Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.

5.  Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.

  * Ha hello alkalmazás jelenik itt meg nem látja, akkor hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját** és set hello **megjelenítése** beállítás túl **Minden alkalmazást.**

6.  Válassza ki az egyszeri bejelentkezés konfigurált hello alkalmazást.

7.  Miután hello alkalmazás betölt, kattintson a hello **egyszeri bejelentkezés** hello alkalmazás bal oldali navigációs menü

8.  A hello **tartomány és az URL-cím** szakaszban, tanulmányozza az hello **megjelenítése speciális URL-beállításainak**.

9.  a hello **azonosító** szövegmezőbe írja be a hello alkalmazás egyedi azonosítója.

10. **Mentés** hello konfigurációs.


**Névfeloldási beállítást 2**

Ha a fenti 1 beállítás nem működik az Ön, távolítsa el hello alkalmazás hello könyvtárból. Adja hozzá, és konfigurálja újra a hello alkalmazás, hajtsa végre az alábbi lépéseket hello:

1.  Nyitott hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**

2.  Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.

3.  Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.

4.  Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.

5.  Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.

  * Ha hello alkalmazás jelenik itt meg nem látja, akkor hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját** és set hello **megjelenítése** beállítás túl **Minden alkalmazást.**

6.  Válassza ki a kívánt tooconfigure egyszeri bejelentkezés hello alkalmazás

7.  Kattintson a **törlése** hello felső – bal oldali hello alkalmazás **áttekintése** panelen.

8.  Frissítse az Azure AD, és adja hozzá a hello alkalmazás hello Azure AD-gyűjteményből. Ezt követően konfigurálja a hello alkalmazás

<span id="_Hlk477190176" class="anchor"></span>Hello alkalmazás újrakonfigurálása, után toohello alkalmazás képes toosign kell lennie.

## <a name="certificate-or-key-not-configured"></a>Tanúsítvány és kulcs nincs konfigurálva

*Hiba AADSTS50003: Nincs aláírási kulcs konfigurálva.*

**Lehetséges ok**

hello objektum sérült, és az Azure AD nem ismeri fel a hello alkalmazáshoz konfigurált hello tanúsítványt.

**Megoldás**

toodelete, és hozzon létre egy új tanúsítványt, kövesse az alábbi hello lépéseket:

1.  Nyitott hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**

2.  Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.

3.  Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.

4.  Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.

5.  Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.

 * Ha hello alkalmazás jelenik itt meg nem látja, akkor hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját** és set hello **megjelenítése** beállítás túl **Minden alkalmazást.**

6.  Válassza ki a kívánt tooconfigure egyszeri bejelentkezés hello alkalmazás

7.  Ha hello alkalmazás betölt, kattintson a hello **egyszeri bejelentkezés** hello alkalmazás bal oldali navigációs menüjében.

8.  Kattintson a **hozzon létre új tanúsítvány** alatt hello **aláíró tanúsítvány SAML** szakasz.

9.  Válassza ki a lejárati dátum. Kattintson a **mentéséhez.**

10. Ellenőrizze **új tanúsítvány aktiválásához** toooverride hello aktív tanúsítványt. Kattintson a **mentése** hello panel hello tetején, és fogadja el a tooactivate hello helyettesítő tanúsítványt.

11. A hello **SAML-aláíró tanúsítványa** területén kattintson **eltávolítása** tooremove hello **nem használt** tanúsítványt.

## <a name="problem-when-customizing-hello-saml-claims-sent-tooan-application"></a>Probléma hello SAML jogcímek testreszabása küldött tooan alkalmazás

toolearn hogyan toocustomize hello SAML attribútum típusú jogcímek küldött tooyour alkalmazás, lásd: [hozzárendelése az Azure Active Directory-jogcímek](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) további információt.

## <a name="next-steps"></a>Következő lépések
[Hogyan toodebug SAML-alapú egyszeri bejelentkezés tooapplications Azure AD-ben](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-debugging)
