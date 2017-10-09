---
title: "az Azure AD aaaManage összevonási tanúsítványok |} Microsoft Docs"
description: "Ismerje meg, hogyan toocustomize hello lejárati dátum összevonási tanúsítványait, és hogyan toorenew tanúsítványokat, amelyeket hamarosan lejár."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: f516f7f0-b25a-4901-8247-f5964666ce23
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/09/2017
ms.author: jeedes
ms.openlocfilehash: e17622d25c9babfa295cc0bb68c24e21b8c574d9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-certificates-for-federated-single-sign-on-in-azure-active-directory"></a>Összevont egyszeri bejelentkezés az Azure Active Directoryban tanúsítványainak kezelése
Ez a cikk ismerteti a gyakori kérdéseket és a vonatkozó információkat, hogy az Azure Active Directory (Azure AD) hoz létre tooestablish toohello tanúsítványok összevont egyszeri bejelentkezés (SSO) tooyour SaaS-alkalmazásokhoz. Hello Azure AD-alkalmazásgyűjtemény vagy egy alkalmazás nem galéria sablon használatával, vegyen fel alkalmazásokat. Hello alkalmazás konfigurálása hello összevont egyszeri bejelentkezési lehetőség használatával.

Ez a cikk, amelyek a konfigurált toouse Azure AD SSO SAML összevonási keresztül kapcsolódó csak tooapps, ahogy az alábbi példa hello:

![Az Azure AD-egyszeri bejelentkezés](./media/active-directory-sso-certs/saml_sso.PNG)

## <a name="auto-generated-certificate-for-gallery-and-non-gallery-applications"></a>Automatikusan létrehozott tanúsítvány gyűjteménye, és nem galéria-alkalmazásokhoz
Új alkalmazás felvétele hello gyűjteményből, és egy SAML-alapú bejelentkezés konfigurálásakor, az Azure AD hello alkalmazás, amely érvényes három évig tanúsítványt hoz létre. Ez a tanúsítvány letölthető hello **SAML-aláíró tanúsítványa** szakasz. Gyűjtemény alkalmazások Ez a szakasz egy beállítás toodownload hello tanúsítványhoz vagy egy metaadatok, attól függően, hogy hello követelmény hello alkalmazás előfordulhat, hogy megjelenítése.

![Az Azure AD egyszeri bejelentkezést.](./media/active-directory-sso-certs/saml_certificate_download.png)

## <a name="customize-hello-expiration-date-for-your-federation-certificate-and-roll-it-over-tooa-new-certificate"></a>Az összevonási tanúsítvány lejárati dátumát hello testreszabása és az új tanúsítvány tooa váltása
Alapértelmezés szerint a tanúsítványok három év után tooexpire állítottak be. Kiválaszthatja a tanúsítvány eltérő lejárati dátummal hello lépések elvégzésével.
hello képernyőképeket hello szakét példában Salesforce használja, de ezeket a lépéseket alkalmazhatja az összevont tooany SaaS-alkalmazás.

1. A hello [Azure-portálon](https://aad.portal.azure.com), kattintson a **vállalati alkalmazás** a hello bal oldali ablaktáblán, és kattintson a **új alkalmazás** a hello **áttekintése** lap:

   ![Nyissa meg hello egyszeri bejelentkezés beállítása varázsló](./media/active-directory-sso-certs/enterprise_application_new_application.png)

2. Keresse meg a hello gyűjtemény alkalmazás, és válassza ki, hogy szeretné-e tooadd hello alkalmazást. Ha hello szükséges alkalmazás nem található, adja hozzá hello alkalmazás hello segítségével **nem-gyűjtemény alkalmazás** lehetőséget. Ez a funkció csak az Azure AD Premium (P1 és P2) SKU hello érhető el.

    ![Az Azure AD egyszeri bejelentkezést.](./media/active-directory-sso-certs/add_gallery_application.png)

3. Kattintson a hello **egyszeri bejelentkezés** hello bal oldali ablaktábla és a Módosítás hivatkozásra **egyszeri bejelentkezés mód** túl**SAML-alapú bejelentkezés**. Ezt a lépést az alkalmazás három év tanúsítványt hoz létre.

4. toocreate egy új tanúsítványt, kattintson a hello **hozzon létre új tanúsítvány** hello hivatkozásra **SAML-aláíró tanúsítványa** szakasz.

    ![Új tanúsítvány létrehozása](./media/active-directory-sso-certs/create_new_certficate.png)

5. Hello **hozzon létre egy új tanúsítványt** a hivatkozás megnyitja hello havinaptár-vezérlőben. Beállíthat bármely dátum és idő toothree év be hello az aktuális dátumot. hello kiválasztva dátum és idő hello új lejárati dátumnak és időpontnak az új tanúsítvány. Kattintson a **Save** (Mentés) gombra.

    ![Töltse le, majd hello-tanúsítvány feltöltése](./media/active-directory-sso-certs/certifcate_date_selection.PNG)

6. Mostantól elérhető toodownload hello új tanúsítványt. Kattintson a hello **tanúsítvány** hivatkozás toodownload azt. Ezen a ponton a tanúsítvány nem lesz aktív. Ha azt szeretné, tooroll toothis tanúsítvány keresztül, válassza ki a hello **új tanúsítvány aktiválásához** jelölőnégyzetet, majd kattintson **mentése**. Ettől a ponttól az Azure AD elindítja hello új tanúsítványt használ az aláíráshoz hello válasz.

7.  toolearn hogyan tooupload hello tanúsítvány tooyour adott SaaS-alkalmazáshoz, kattintson a hello **nézet konfigurációs oktatóanyag** hivatkozásra.

## <a name="renew-a-certificate-that-will-soon-expire"></a>Rövidesen lejáró tanúsítvány megújítása
hello lépések megújítási kell nincs jelentős állásidőt eredményezhettek a felhasználók számára. Ez a szakasz a szolgáltatás Salesforce példaként, de ezeket a lépéseket a hello képernyőképeket összevont tooany SaaS-alkalmazás is alkalmazhatja.

1. A hello **Azure Active Directory** alkalmazás **egyszeri bejelentkezés** lapján hello új tanúsítványt az alkalmazáshoz létrehozni. Ehhez kattintson a hello **hozzon létre új tanúsítvány** hello hivatkozásra **SAML-aláíró tanúsítványa** szakasz.

    ![Új tanúsítvány létrehozása](./media/active-directory-sso-certs/create_new_certficate.png)

2. SELECT hello lejárati dátumnak és időpontnak a új tanúsítványt, és kattintson a kívánt **mentése**.

3. Töltse le a hello hello tanúsítványt **SAML-aláíró tanúsítványa** lehetőséget. Töltse fel a hello új tanúsítvány toohello SaaS-alkalmazáshoz tartozó egyszeri bejelentkezés konfigurálására szolgáló képernyőn. toolearn hogyan toodo ezt az adott SaaS-alkalmazáshoz, kattintson a hello **nézet konfigurációs oktatóanyag** hivatkozásra.
   
4. tooactivate hello új tanúsítványt az Azure AD-válassza hello **új tanúsítvány aktiválásához** négyzet jelölését, majd kattintson a hello **mentése** hello oldal hello tetején gombra. Ez áthalad hello új tanúsítványt a hello Azure AD oldalán. hello hello tanúsítvány állapota a **új** túl**aktív**. Ettől a ponttól az Azure AD elindítja hello új tanúsítványt használ az aláíráshoz hello válasz. 
   
    ![Új tanúsítvány létrehozása](./media/active-directory-sso-certs/new_certificate_download.png)

## <a name="related-articles"></a>Kapcsolódó cikkek
* [Hogyan kapcsolatos bemutatók felsorolása toointegrate SaaS-alkalmazásokhoz az Azure Active Directoryval](active-directory-saas-tutorial-list.md)
* [Alkalmazások kezelése az Azure Active Directoryban cikk indexe](active-directory-apps-index.md)
* [Alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md)
* [Hibaelhárítási SAML-alapú egyszeri bejelentkezést.](active-directory-saml-debugging.md)
