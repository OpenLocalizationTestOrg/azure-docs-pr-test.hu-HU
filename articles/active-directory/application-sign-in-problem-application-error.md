---
title: "a bejelentkezés után az alkalmazás oldalon aaaError |} Microsoft Docs"
description: "Hogyan tooresolve problémák az Azure ad-val során a bejelentkezéshez hello alkalmazás maga bocsát ki a hiba"
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
ms.openlocfilehash: 317b6f8e6417520ead80ae4e26c591ba6b134683
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="error-on-an-applications-page-after-signing-in"></a>Hiba történt a bejelentkezés után az alkalmazás az oldalon

Ebben a forgatókönyvben az Azure AD hello felhasználó írt alá, de hello alkalmazás nem teszi lehetővé hello felhasználói toosuccessfully Befejezés hello bejelentkezési folyamata hibát jelenít meg. Ebben a forgatókönyvben hello alkalmazás nem fogad el hello válasz probléma az Azure ad.

Ennek oka néhány miért hello alkalmazás nem fogadta el az Azure AD hello válaszát. Ha hello hiba hello alkalmazásban nem elég egyértelmű tooknow Mi hiányzik hello válaszként, majd:

-   Az Azure AD hello gyűjteménye hello alkalmazás esetén ellenőrizze a hello cikkben minden hello lépésekkel [hogyan toodebug SAML-alapú egyszeri bejelentkezés tooapplications az Azure Active Directoryban](https://azure.microsoft.com/documentation/articles/active-directory-saml-debugging).

-   Egy eszköz, például használja [Fiddler](http://www.telerik.com/fiddler) toocapture SAML kérte, SAML-válasz és SAML-jogkivonat.

-   Közös hello SAML-válasz hello alkalmazás szállító tooknow Mi hiányzik.

## <a name="missing-attributes-in-hello-saml-response"></a>Hiányzó attribútumok hello SAML-válasz

tooadd hello Azure AD konfigurációs toobe hello Azure AD válaszként küldött attribútum hello lépéseket kövesse:

1.  Nyitott hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**

2.  Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.

3.  Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.

4.  Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.

5.  Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.

   * Ha hello alkalmazás jelenik itt meg nem látja, akkor hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját** és set hello **megjelenítése** beállítás túl **Minden alkalmazást.**

6.  Válassza ki a kívánt tooconfigure egyszeri bejelentkezés hello alkalmazást.

7.  Ha hello alkalmazás betölt, kattintson a hello **egyszeri bejelentkezés** hello alkalmazás bal oldali navigációs menüjében.

8.  Kattintson a **megtekintés és Szerkesztés minden más felhasználói attribútumok alapján** hello **felhasználói attribútumok** szakasz tooedit hello attribútumok küldött toobe toohello alkalmazás hello SAML-jogkivonat felhasználói bejelentkezéskor.

   tooadd attribútum:

   * Kattintson a **Hozzáadás attribútum**. Adja meg a hello **neve** és hello válassza hello **érték** hello legördülő menüből.

   * Kattintson a **mentéséhez.** Új attribútum hello hello táblázatban láthatja.

9.  Hello konfigurációjának mentéséhez.

Hello felhasználó jelentkezik be toohello alkalmazást, amikor legközelebb az Azure AD küldenek hello új attribútum hello SAML-válasz.

## <a name="hello-application-expects-a-different-user-identifier-value-or-format"></a>hello alkalmazás egy másik felhasználói azonosító értéke vagy a format vár

hello bejelentkezési toohello alkalmazás sikertelen, mert hello SAML-válasz attribútumok például a szerepkörök hiányzik, vagy hello alkalmazás által várt paraméterekkel hello entityid beállítást attribútum formátumát.

## <a name="add-an-attribute-in-hello-azure-ad-application-configuration"></a>Attribútum hozzáadása a hello Azure AD alkalmazás beállításai:

toochange hello felhasználói azonosító értéke, kövesse a hello alábbi lépéseket:

1.  Nyitott hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**

2.  Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.

3.  Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.

4.  Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.

5.  Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.

   * Ha hello alkalmazás jelenik itt meg nem látja, akkor hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját** és set hello **megjelenítése** beállítás túl **Minden alkalmazást.**

6.  Válassza ki a kívánt tooconfigure egyszeri bejelentkezés hello alkalmazást.

7.  Ha hello alkalmazás betölt, kattintson a hello **egyszeri bejelentkezés** hello alkalmazás bal oldali navigációs menüjében.

8.  A hello **felhasználói attribútumok**, válassza ki a felhasználók a hello egyedi azonosítója hello **felhasználói azonosító** legördülő menüből.

## <a name="change-entityid-user-identifier-format"></a>Entityid (felhasználói azonosító) beállítást formátumának módosítása

Ha hello alkalmazás hello entityid beállítást attribútum egy másik formátumát vár. Majd nem fogja tudni tooselect hello entityid (felhasználói azonosító) beállítást formátuma, hogy az Azure AD küld toohello alkalmazás hello válaszul felhasználói hitelesítés után.

Az Azure AD válassza hello formátumban hello NameID attribútum (felhasználói azonosító) kijelölt hello érték alapján, vagy a SAML AuthRequest hello hello alkalmazás által kért formátum hello. További információt a Microsoft hello cikk [egyszeri bejelentkezés SAML protokoll](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) az hello rész NameIDPolicy.

## <a name="hello-application-expects-a-different-signature-method-for-hello-saml-response"></a>hello alkalmazás vár egy eltérő aláírással módszer, amellyel hello SAML-válasz

Azure Active Directory által digitálisan aláírt hello SAML-jogkivonat részeinek toochange. Kövesse az alábbi hello lépéseket:

1.  Nyitott hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**

2.  Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.

3.  Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.

4.  Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.

5.  Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.

  * Ha hello alkalmazás jelenik itt meg nem látja, akkor hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját** és set hello **megjelenítése** beállítás túl **Minden alkalmazást.**

6.  Válassza ki a kívánt tooconfigure egyszeri bejelentkezés hello alkalmazást.

7.  Ha hello alkalmazás betölt, kattintson a hello **egyszeri bejelentkezés** hello alkalmazás bal oldali navigációs menüjében.

8.  Kattintson a **megjelenítése speciális tanúsítvány-aláírási beállítások** alatt hello **SAML-aláíró tanúsítványa** szakasz.

9.  Jelölje be hello megfelelő **aláíró beállítás** hello alkalmazás által várt:

  * Bejelentkezési SAML-válasz

  * Bejelentkezési SAML-válasz és helyességi feltétel

  * Bejelentkezési SAML-előfeltétel

Hello felhasználó jelentkezik be toohello alkalmazást, amikor legközelebb az Azure AD bejelentkezési hello hello SAML-válasz kijelölt részét.

## <a name="hello-application-expects-hello-signing-algorithm-toobe-sha-1"></a>hello alkalmazást aláíró algoritmus toobe SHA-1 hello vár

Alapértelmezés szerint az Azure AD aláírja a hello SAML-jogkivonat hello használata a legtöbb biztonsági algoritmus. Hello bejelentkezési algoritmus tooSHA-1 módosítása nem ajánlott, kivéve, ha hello alkalmazáshoz szükséges.

toochange aláírási algoritmus hello hajtsa végre az alábbi hello lépéseket:

1.  Nyitott hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**

2.  Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.

3.  Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.

4.  Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.

5.  Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.

   * Ha hello alkalmazás jelenik itt meg nem látja, akkor hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját** és set hello **megjelenítése** beállítás túl **Minden alkalmazást.**

6.  Válassza ki a kívánt tooconfigure egyszeri bejelentkezés hello alkalmazást.

7.  Ha hello alkalmazás betölt, kattintson a hello **egyszeri bejelentkezés** hello alkalmazás bal oldali navigációs menüjében.

8.  Kattintson a **megjelenítése speciális tanúsítvány-aláírási beállítások** alatt hello **SAML-aláíró tanúsítványa** szakasz.

9.  Válassza ki az SHA-1, a hello **aláírási algoritmus**.

Amikor legközelebb hello toohello alkalmazás felhasználó bejelentkezik, az Azure AD bejelentkezési hello SHA-1 algoritmussal SAML-jogkivonat.

## <a name="next-steps"></a>Következő lépések
[Hogyan toodebug SAML-alapú egyszeri bejelentkezés tooapplications az Azure Active Directoryban](https://azure.microsoft.com/documentation/articles/active-directory-saml-debugging)
