---
title: "összevont egyszeri bejelentkezés nem galéria alkalmazások konfigurálása aaaProblem |} Microsoft Docs"
description: "Néhány hello gyakori problémák jelentkezhetnek, ha összevont egyszeri bejelentkezés tooyour egyéni SAML-alkalmazások konfigurálása, amely nem szerepel az Azure AD Application Gallery hello cím"
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
ms.openlocfilehash: 8c80f0001de01cbc9930ef0441cd804082ee8578
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="problem-configuring-federated-single-sign-on-for-a-non-gallery-application"></a>A probléma összevont egyszeri bejelentkezés nem galéria alkalmazások konfigurálása

Ha probléma merül fel, ha az alkalmazások konfigurálásáról. Ellenőrizze, hogy minden hello lépésekkel hello cikkben [konfigurálása egyszeri bejelentkezéshez tooapplications, amelyek hello Azure Active Directory alkalmazáskatalógusában nem találhatók.](https://docs.microsoft.com/azure/active-directory/active-directory-saas-custom-apps)

## <a name="cant-add-another-instance-of-hello-application"></a>Hello alkalmazás egy másik példánya nem vehető fel.

tooadd egy második alkalmazáspéldányt, kell toobe képes:

-   Konfigurálja a második példány hello egyedi azonosítója. Nem fog tudni tooconfigure hello hello első példánynál használt ugyanezzel az azonosítóval.

-   Egy másik tanúsítványt, mint egy hello elsősorban a hello konfigurálása.

Ha hello alkalmazás nem támogatja a fenti hello bármelyikét. Majd nem fogja tudni tooconfigure egy második példányt.

## <a name="where-do-i-set-hello-entityid-user-identifier-format"></a>Ahol a hello entityid (felhasználói azonosító) beállítást formátum beállítása

Nem lehet, hogy az Azure AD küld toohello alkalmazás hello válaszul felhasználói hitelesítés után képes tooselect hello entityid beállítást (felhasználói azonosító) formátumban.

Az Azure AD válassza hello formátumban hello NameID attribútum (felhasználói azonosító) kijelölt hello érték alapján, vagy a SAML AuthRequest hello hello alkalmazás által kért formátum hello. További információt a Microsoft hello cikk [egyszeri bejelentkezés SAML protokoll](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) hello szakaszban NameIDPolicy,

## <a name="where-do-i-get-hello-application-metadata-or-certificate-from-azure-ad"></a>Hol hello alkalmazás metaadatainak vagy a tanúsítvány hozzá az Azure AD

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
