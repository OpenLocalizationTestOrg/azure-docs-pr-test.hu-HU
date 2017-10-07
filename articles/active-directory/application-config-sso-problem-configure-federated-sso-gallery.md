---
title: "összevont egyszeri bejelentkezés az Azure AD-katalógusában alkalmazás konfigurálása aaaProblem |} Microsoft Docs"
description: "Cím néhány hello gyakori problémák jelentkezhetnek, ha konfigurálása összevont egyszeri bejelentkezés alkalmazásokhoz az Azure AD Application Gallery hello szereplő SAML használatával"
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
ms.openlocfilehash: 2ae1e511bd49b19159e1ab83cf79a7db5403b9a1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="problem-configuring-federated-single-sign-on-for-an-azure-ad-gallery-application"></a>A probléma összevont egyszeri bejelentkezés az Azure AD-katalógusában alkalmazás konfigurálása

Ha probléma merül fel, ha az alkalmazások konfigurálásáról. Ellenőrizze, hogy minden hello lépéseket követte hello oktatóprogram hello alkalmazás. Hello alkalmazást, a beágyazott dokumentációja hogyan tooconfigure hello alkalmazás van. Emellett érheti el hello [kapcsolatos bemutatók felsorolása toointegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/) kapcsolatos részletek részletes útmutatás.

## <a name="cant-add-another-instance-of-hello-application"></a>Hello alkalmazás egy másik példánya nem vehető fel.

tooadd egy második alkalmazáspéldányt, kell toobe képes:

-   Konfigurálja a második példány hello egyedi azonosítója. Nem fog tudni tooconfigure hello hello első példánynál használt ugyanezzel az azonosítóval.

-   Egy másik tanúsítványt, mint egy hello elsősorban a hello konfigurálása.

Ha hello alkalmazás nem támogatja a fenti hello bármelyikét. Majd nem fogja tudni tooconfigure egy második példányt.

## <a name="cant-add-hello-identifier-or-hello-reply-url"></a>Nem adható hozzá hello azonosító, vagy nem hello válasz URL-címe

Ha nem tudja tooconfigure hello azonosítója vagy hello válasz URL-CÍMEN, erősítse meg a hello azonosítója, és válasz URL-CÍMEN megegyeznek hello minták hello alkalmazás előre konfigurálva van.

tooknow hello minták hello alkalmazás előre konfigurálva:

1.  Nyitott hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.** Nyissa meg toostep 7. Ha már hello alkalmazás konfigurációs panel az Azure ad-val.

2.  Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.

3.  Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.

4.  Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.

5.  Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.

   * Ha hello alkalmazás jelenik itt meg nem látja, akkor hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját** és set hello **megjelenítése** beállítás túl **Minden alkalmazást.**

6.  Válassza ki a kívánt tooconfigure egyszeri bejelentkezés hello alkalmazást.

7.  Ha hello alkalmazás betölt, kattintson a hello **egyszeri bejelentkezés** hello alkalmazás bal oldali navigációs menüjében.

8.  Válassza ki **SAML-alapú bejelentkezés** a hello **mód** legördülő menüből.

9.  Nyissa meg toohello **azonosítója** vagy **válasz URL-CÍMEN** szövegmező alatt hello **tartomány és az URL-címeket.**

10. Háromféleképpen tooknow hello támogatott minták hello alkalmazáshoz:

   * Hello szövegmezőjének helyőrzőként látja hello támogatott példány szükséges *példa:* <https://contoso.com>.

   * hello nem módosíthatók, ha egy piros felkiáltójel látja hello szövegmezőjének tooenter hello értéke meg. Ha az egérmutatóval rámutat hello piros felkiáltójel, lásd: hello támogatott kombinációját.

   * Hello alkalmazás hello oktatóanyagban is kaphat a hello támogatott mintájával kapcsolatos információkért. A hello **az Azure AD konfigurálása egyszeri bejelentkezéshez** szakasz. Nyissa meg toohello lépés konfigurált hello értékek alapján hello **tartomány és az URL-címek** szakasz.

Ha hello értékek nem egyeznek meg hello mintákat előre konfigurált Azure ad-val. A következőket teheti:

-   Hello alkalmazás szállító tooget értékek, amelyek megfelelnek az Azure ad-val előre konfigurált hello minta használata

-   Másik lehetőségként felveheti a kapcsolatot az Azure AD-csapatának a < aadapprequest@microsoft.com > vagy hagyja üresen a Megjegyzés hello oktatóanyag toorequest hello frissítés hello alkalmazás hello támogatott minták

## <a name="where-do-i-set-hello-entityid-user-identifier-format"></a>Ahol a hello entityid (felhasználói azonosító) beállítást formátum beállítása

Nem lehet, hogy az Azure AD küld toohello alkalmazás hello válaszul felhasználói hitelesítés után képes tooselect hello entityid beállítást (felhasználói azonosító) formátumban.

Az Azure AD válassza hello formátumban hello NameID attribútum (felhasználói azonosító) kijelölt hello érték alapján, vagy a SAML AuthRequest hello hello alkalmazás által kért formátum hello. További információt a Microsoft hello cikk [egyszeri bejelentkezés SAML protokoll](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) hello szakaszban NameIDPolicy,

## <a name="cant-find-hello-azure-ad-metadata-toocomplete-hello-configuration-with-hello-application"></a>Nem található a hello Azure Active Directory metaadatok toocomplete hello beállítása hello alkalmazással

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

## <a name="dont-know-how-toocustomize-saml-claims-sent-tooan-application"></a>Nem tudjuk, hogyan toocustomize SAML jogcímek küldött tooan alkalmazás

toolearn hogyan toocustomize hello SAML attribútum típusú jogcímek küldött tooyour alkalmazás, lásd: [hozzárendelése az Azure Active Directory-jogcímek](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) további információt.

## <a name="next-steps"></a>Következő lépések
[Alkalmazások kezelése az Azure Active Directoryban](active-directory-enable-sso-scenario.md)
