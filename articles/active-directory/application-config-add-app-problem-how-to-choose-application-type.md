---
title: "melyik alkalmazás toouse írja be, amikor egy alkalmazás hozzáadása aaaHow toochoose |} Microsoft Docs"
description: "Hello támogatott ismerteti az alkalmazások integrálva az Azure AD és a kapcsolódó konfigurációs beállítások"
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
ms.openlocfilehash: 46e3672e7f5048b0fa54171f0fc169362c9d5ac6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toochoose-which-application-type-toouse-when-adding-an-application"></a>Hogyan toochoose melyik alkalmazás toouse írja be, amikor egy alkalmazás hozzáadása

Ez a cikk segítséget toounderstand hello négy fő típusú alkalmazások integrálható az Azure AD:

* Támogatott források és műveletek szerint
* Miért akkor érdemes választania, melyik alkalmazás
* Hogyan tooconfigure ezen alkalmazás core tulajdonságai, például a felhasználók **kiépített**, vagy mi **egyszeri bejelentkezés** technológia toouse.

## <a name="supported-application-types-in-azure-ad"></a>Az Azure ad-ben támogatott alkalmazástípusok

Az Azure AD által támogatott négy fő típusokat adhat hozzá hello **Hozzáadás** szolgáltatás alatt található **vállalati alkalmazások**. Ezek a következők:

-   **Azure AD-gyűjtemény alkalmazások** –, amelyet az egyszeri bejelentkezés az Azure ad-vel előre integrált alkalmazás.

-   **Alkalmazás Proxy alkalmazások** – a helyszíni környezetben futó tooprovide biztonságos egyszeri bejelentkezést tooexternally kívánt alkalmazáshoz.

-   **Egyéni által fejlesztett alkalmazások** – olyan alkalmazás, amely a szervezet által toodevelop a hello Azure AD alkalmazás-fejlesztő Platform, de, amely nem még létezik.

-   **Nem-gyűjtemény alkalmazások** – kapcsolja a saját alkalmazásai! Bármely kívánt webes hivatkozást, vagy egy felhasználónevet és jelszót a mezőben, amely alkalmazás SAML vagy az OpenID Connect protokollt támogatja, vagy az egyszeri bejelentkezés az Azure ad-val toointegrate verzióváltáshoz SCIM támogatja.

## <a name="features-and-capabilities-supported-by-all-hello-above-application-types"></a>Funkciók és képességek összes hello fent alkalmazástípusok által támogatott

hello következő szolgáltatások támogatottak bármelyik fenti 4 alkalmazástípusok hello Azure AD-ben:

-   **Gyors üzembe helyezési** – induláshoz alkalmazással gyorsan következő [egyszerű üzembe helyezés lépései](https://docs.microsoft.com/azure/active-directory/active-directory-integrating-applications-getting-started)

-   **Általános tulajdonságok felügyeleti** – beszerzése egy [közvetlen mélyhivatkozás](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#deploying-azure-ad-integrated-applications-to-users) tooan alkalmazás [hello branding testreszabása](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-change-app-logo-user-azure-portal) egy alkalmazás vagy [hello alkalmazásletiltása](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-disable-app-azure-portal) az összes felhasználó számára.

-   **Felhasználók és csoportok kezelése** – [hozzárendelése](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal) vagy [eltávolítása](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-remove-assignment-azure-portal) felhasználók és csoportok tooan alkalmazás, és szükség esetén hozzárendelése hello az adott alkalmazást szerepkörök a felhasználók és csoportok hozzáférése

-   **Önkiszolgáló alkalmazás-hozzáférés** – engedélyezése a felhasználók toorequest [önkiszolgáló alkalmazás-hozzáférés](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) tooan alkalmazás az alkalmazás-hozzáférés a panelek vagy egy alkalmazás közvetlenül hozzáadásával vagy [ Csatlakozás egy önkiszolgáló engedélyezett csoporthoz](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-self-service-group-management), opcionálisan mentén hello üzleti jóváhagyásra van szükség módja

-   **Bejelentkezési naplók** – lásd: [összes hello bejelentkezések tooan alkalmazás](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-sign-ins), vagy az alkalmazások

-   **A naplók** – lásd: [naplók módosítások tooan alkalmazással kapcsolatos részletes](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs), vagy tooall az alkalmazások

-   **Feltételes és kockázati-alapú hozzáférés** – hatékony beállított [feltétel-alapú hozzáférési szabályok](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access) , amely akkor lépnek érvénybe, ha a felhasználó toosign tooa adott alkalmazásban

-   **Engedélyek megtekintése** – hello bármelyikét megtekintése [OAuth2 engedélyek](https://docs.microsoft.com/azure/active-directory/active-directory-apps-permissions-consent) alkalmazás rendelkezik hozzáféréssel tooin a címtár egyetlen helyéről

## <a name="single-sign-on-and-provisioning-modes-supported-by-specific-application-types"></a>Egyszeri bejelentkezés és a létesítési módot támogat. az adott alkalmazást típusok

hello az alábbi táblázat hello különböző egyszeri bejelentkezést és a létesítési módot támogat az egyes alkalmazástípusok fent hello által. Használhatja a tábla toohelp toounderstand, melyik alkalmazás szüksége tooadd toosupport adott cél.

  ![Alkalmazás típusú tábla](./media/application-tables/table1.png)

## <a name="how-toochoose-a-single-sign-on-mode"></a>Hogyan toochoose a egyszeri bejelentkezés módját

hello támogatott **egyszeri bejelentkezés** módjainak Azure AD-alkalmazások az alábbiak.

-   **Az Azure AD az egyszeri bejelentkezés le van tiltva** – válassza ki az Azure AD az egyszeri bejelentkezés le van tiltva **egyszeri bejelentkezés mód** Ha még nem áll készen toointegrate ehhez az alkalmazáshoz az egyszeri bejelentkezés az Azure ad-vel, vagy egyszerűen csak tesztelés kimenő

-   **Bejelentkezés kapcsolódó** – hello válasszon [társított bejelentkezés](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) **egyszeri bejelentkezés mód** Ha valamelyik alkalmazásnál, amely a már meglévő egyszeri bejelentkezési megoldással már csatlakoztatva van, vagy ha csak a felhasználók a hivatkozás egy egyszerű toopublish azok [alkalmazás hozzáférési Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) vagy [Office 365 alkalmazásindító](https://login.microsoftonline.com/common/oauth2/authorize?response_mode=form_post&response_type=id_token&scope=openid&nonce=d508a995-f6d6-4b8a-81b8-825c71f1be46.636253878097046923&state=https%3a%2f%2fsupport.office.com%2farticle%2fMeet-the-Office-365-app-launcher-79f12104-6fed-442f-96a0-eb089a3f476a%3fui%3den-US%26rs%3den-US%26ad%3dUS&client_id=4b233688-031c-404b-9a80-a4f3f2351f90&redirect_uri=https%3a%2f%2fsupport.office.com%2fauth%2fsignin&login_hint=asteen%40microsoft.com&prompt=none)

-   **Jelszó alapú bejelentkezés** – hello válasszon [jelszóalapú bejelentkezés](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) **egyszeri bejelentkezés mód** Ha az alkalmazás Renderelés egy HTML-felhasználónév és jelszó mezőt, és azt szeretné, hogy toostore, felhasználónév és jelszó biztonságosan toobe újból később toohello alkalmazás

-   **SAML-alapú bejelentkezés** – hello válasszon [SAML-alapú bejelentkezés](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) mód, ha az alkalmazás hello SAML vagy az OpenID Connect protokollt támogatja, vagy azt szeretné, hogy toobe képes toomap felhasználók egyszeri bejelentkezés toospecific alkalmazás szerepkörök alapján a SAML meghatározott szabályok jogcímek *

   >[!NOTE]
   >Ez a beállítás nem érhető el, ha hello alkalmazásproxy úgy van konfigurálva, az alkalmazás.
   >
   >

-   **Fejléc-alapú bejelentkezés** – válassza ezt a [fejléc-alapú bejelentkezés](https://docs.microsoft.com/azure/active-directory/application-proxy-ping-access#what-is-pingaccess-for-azure-ad) egyetlen bejelentkezés módja Ha valamely alkalmazás használatával, amely támogatja a HTTP-fejléc PingAccess alapján kívánja tooperform egyszeri bejelentkezést a túl hitelesítés

   >[!NOTE]
   >Ez a beállítás csak érhető el, ha hello proxy és PingAccess úgy van konfigurálva, az alkalmazás.
   >
   >

-   **Integrált Windows-hitelesítés** – hello válasszon [integrált Windows-hitelesítés](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-sso-using-kcd) túl a kívánja tooperform egyszeri bejelentkezést a helyszíni WIA alkalmazás kitettségének üzemmódot egyszeri bejelentkezés

   >[!NOTE]
   >Ez a beállítás csak érhető el, ha hello alkalmazásproxy úgy van konfigurálva, az alkalmazás.
   >
   >

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

6.  Válassza ki legyen tooconfigure egyszeri bejelentkezés hello alkalmazást.

7.  Amikor hello alkalmazás betölt, kattintson a **egyszeri bejelentkezés** hello alkalmazás bal oldali navigációs menüjében.

## <a name="how-toochoose-a-provisioning-mode"></a>Hogyan toochoose a létesítés

-   **Manuális létesítési** – hello válasszon [manuális](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-manage-provisioning#provisioning-modes) kiépítési üzemmódban, ha meglévő fiókokat, vagy kívül az Azure AD alkalmazás toomanage fiókok kívánja.

-   **Automatikus kiépítés** – hello válasszon [automatikus](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-manage-provisioning#configuring-automatic-user-account-provisioning) **létesítés** felhasználói fiókot toothis, ha azt szeretné, hogy tooenable automatikus API-alapú üzembe helyezés és/vagy a megszüntetést alkalmazás 

   >[!NOTE]
   >Ez a beállítás csak hello alkalmazást érhető **kiemelt** hello kategóriáját [az Azure AD Application Gallery](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-whats-new-azure-portal#the-new-and-improved-application-gallery).
   >
   >

-   **SCIM-alapú automatikus üzembe helyezés** – használható [SCIM-alapú automatikus üzembe helyezés](https://docs.microsoft.com/azure/active-directory/active-directory-scim-provisioning) Ha az alkalmazás támogatja-e a módosítások toousers és a csoport, amelyek a módosítások automatikusan kibocsátott hello SCIM protokoll az Azure ad-vel integrált tooany alkalmazás 

   >[!NOTE]
   >Ez a beállítás nem szerepel egy üzembe helyezési mód, de minden olyan alkalmazásnál, az Azure ad-vel integrált alapértelmezés szerint engedélyezve van.
   >
   >

## <a name="how-tooset-an-applications-provisioning-mode"></a>Hogyan a tooset az alkalmazás csomagazonosítóját kiépítési üzemmódban

tooset alkalmazás **kiépítés** mód, kövesse az alábbi hello utasítások:

tooset alkalmazás **egyszeri bejelentkezés** mód, kövesse az alábbi hello utasítások:

1.  Nyitott hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**

2.  Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.

3.  Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.

4.  Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.

5.  Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.

  * Ha hello alkalmazás jelenik itt meg nem látja, akkor hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját** és set hello **megjelenítése** beállítás túl **Minden alkalmazást.**

6.  Válassza ki a kívánt tooconfigure kiépítés hello alkalmazást.

7.  Amikor hello alkalmazás betölt, kattintson a **kiépítési** hello alkalmazás bal oldali navigációs menüjében.

## <a name="next-steps"></a>Következő lépések
[Alkalmazások kezelése az Azure Active Directoryban](active-directory-enable-sso-scenario.md)
