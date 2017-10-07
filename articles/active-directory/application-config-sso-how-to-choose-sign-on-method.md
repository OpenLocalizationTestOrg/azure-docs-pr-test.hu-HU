---
title: "aaaHow toodetermine milyen egyszeri bejelentkezést metódus toouse |} Microsoft Docs"
description: "Hello egyszeri bejelentkezés módot támogat az Azure AD megértése és hogyan toopick, mely egy toochoose az alkalmazás hello szüksége."
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
ms.openlocfilehash: 64f0bc1dc8281d1ab8222fd50eaceaf710704886
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toodetermine-what-single-sign-on-method-toouse"></a>Hogyan toodetermine milyen egyszeri bejelentkezést metódus toouse

Ez a cikk segítséget toounderstand hello egyszeri bejelentkezés módot támogat az Azure AD és hogyan toopick, mely egy toochoose az alkalmazás hello szüksége.

## <a name="single-sign-on-and-provisioning-modes-supported-by-specific-application-types"></a>Egyszeri bejelentkezés és a létesítési módot támogat. az adott alkalmazást típusok

hello az alábbi táblázat hello különböző egyszeri bejelentkezést és a létesítési módot támogat az egyes alkalmazástípusok fent hello által. Használhatja a tábla toohelp toounderstand, melyik alkalmazás szüksége tooadd toosupport adott cél.

  ![Ázsia és a csendes típusai tábla](./media/application-tables/table1.png)

## <a name="how-toochoose-a-single-sign-on-mode"></a>Hogyan toochoose a egyszeri bejelentkezés módját

hello támogatott **egyszeri bejelentkezés** módjainak Azure AD-alkalmazások az alábbiak.

-   **Az Azure AD az egyszeri bejelentkezés le van tiltva** – válassza ki az Azure AD az egyszeri bejelentkezés le van tiltva **egyszeri bejelentkezés mód** Ha még nem áll készen toointegrate ehhez az alkalmazáshoz az egyszeri bejelentkezés az Azure ad-vel, vagy egyszerűen csak tesztelés kimenő

-   **Bejelentkezés kapcsolódó** – hello válasszon [társított bejelentkezés](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) **egyszeri bejelentkezés mód** Ha valamelyik alkalmazásnál, amely a már meglévő egyszeri bejelentkezési megoldással már csatlakoztatva van, vagy ha csak a felhasználók a hivatkozás egy egyszerű toopublish azok [alkalmazás hozzáférési Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) vagy [Office 365 alkalmazásindító](https://login.microsoftonline.com/common/oauth2/authorize?response_mode=form_post&response_type=id_token&scope=openid&nonce=d508a995-f6d6-4b8a-81b8-825c71f1be46.636253878097046923&state=https%3a%2f%2fsupport.office.com%2farticle%2fMeet-the-Office-365-app-launcher-79f12104-6fed-442f-96a0-eb089a3f476a%3fui%3den-US%26rs%3den-US%26ad%3dUS&client_id=4b233688-031c-404b-9a80-a4f3f2351f90&redirect_uri=https%3a%2f%2fsupport.office.com%2fauth%2fsignin&login_hint=asteen%40microsoft.com&prompt=none)

-   **Jelszó alapú bejelentkezés** – hello válasszon [jelszóalapú bejelentkezés](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) **egyszeri bejelentkezés mód** Ha az alkalmazás Renderelés egy HTML-felhasználónév és jelszó mezőt, és azt szeretné, hogy toostore, felhasználónév és jelszó biztonságosan toobe újból később toohello alkalmazás

-   **SAML-alapú bejelentkezés** – hello válasszon [SAML-alapú bejelentkezés](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) mód, ha az alkalmazás hello SAML vagy az OpenID Connect protokollt támogatja, vagy azt szeretné, hogy toobe képes toomap felhasználók egyszeri bejelentkezés toospecific alkalmazás szerepkörök alapján a SAML meghatározott szabályok jogcímek *(**Megjegyzés:** Ez a beállítás nem érhető el, ha hello alkalmazásproxy úgy van konfigurálva, az alkalmazás) *

-   **Fejléc-alapú bejelentkezés** – válassza ezt a [fejléc-alapú bejelentkezés](https://docs.microsoft.com/azure/active-directory/application-proxy-ping-access#what-is-pingaccess-for-azure-ad) egyetlen bejelentkezés módja Ha valamely alkalmazás használatával, amely támogatja a HTTP-fejléc PingAccess alapján kívánja tooperform egyszeri bejelentkezést a túl hitelesítés *(**Megjegyzés:** ezt a beállítást csak érhető el, ha egy alkalmazás hello proxy és PingAccess van konfigurálva) *

-   **Integrált Windows-hitelesítés** – hello válasszon [integrált Windows-hitelesítés](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-sso-using-kcd) túl a kívánja tooperform egyszeri bejelentkezést a helyszíni WIA alkalmazás kitettségének üzemmódot egyszeri bejelentkezés*()*  *Megjegyzés:** ezt a beállítást csak érhető el, ha egy alkalmazás hello alkalmazásproxy van konfigurálva) *

## <a name="single-sign-on-modes-for-custom-developed-applications"></a>Egyszeri bejelentkezés módjainak egyéni által fejlesztett alkalmazások

Egyéni hello használatával fejlesztett alkalmazások rendelkeznek [egyéni által fejlesztett alkalmazás](#_Custom-Developed_Applications) élmény is további egyszeri bejelentkezés módot támogat fent nem említett. Ezek a következők:

-   [OAuth 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-oauth-code) bejelentkezés alapján

-   [OpenID Connect 1.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-openid-connect-code) bejelentkezés alapján

-   [WS-Federation 1.2](http://docs.oasis-open.org/wsfed/federation/v1.2/os/ws-federation-1.2-spec-os.html) bejelentkezés alapján

-   [SAML 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-protocol-reference) bejelentkezés alapján

Olvasási hello [Azure Active Directory fejlesztői útmutatója](https://docs.microsoft.com/azure/active-directory/develop/active-directory-developers-guide) toolearn hogyan egyéni által fejlesztett toocreate alkalmazást, amely támogatja ezeket egyszeri bejelentkezési módok többet.

## <a name="how-tooset-an-applications-single-sign-on-mode"></a>Hogyan tooset alkalmazás csomagazonosítóját egyszeri bejelentkezés mód

tooset alkalmazás **egyszeri bejelentkezés** mód, kövesse az alábbi hello utasítások:

1.  Nyitott hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**

2.  Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.

3.  Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.

4.  Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.

5.  Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.

   * Ha hello alkalmazás jelenik itt meg nem látja, akkor hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját** és set hello **megjelenítése** beállítás túl **Minden alkalmazást.**

6.  Válassza ki a kívánt tooconfigure egyszeri bejelentkezés hello alkalmazás

7.  Amikor hello alkalmazás betölt, kattintson a **egyszeri bejelentkezés** hello alkalmazás bal oldali navigációs menüjében.

## <a name="next-steps"></a>Következő lépések
[Adja meg az egyszeri bejelentkezés tooyour alkalmazások alkalmazásproxyval](active-directory-application-proxy-sso-using-kcd.md)

