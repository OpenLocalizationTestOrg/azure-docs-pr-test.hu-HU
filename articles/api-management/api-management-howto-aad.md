---
title: "aaaAuthorize fejlesztői fiókok Azure Active Directory - Azure API Management használatával |} Microsoft Docs"
description: "Ismerje meg, hogy az Azure Active Directoryval az API Management tooauthorize felhasználók."
services: api-management
documentationcenter: API Management
author: steved0x
manager: erikre
editor: 
ms.assetid: 33a69a83-94f2-4e4e-9cef-f2a5af3c9732
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: ebf5447a509a47df35e4262138bfcf423cb1dd5c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooauthorize-developer-accounts-using-azure-active-directory-in-azure-api-management"></a>Hogyan tooauthorize fejlesztői fiókok Azure Active Directory, az Azure API Management
## <a name="overview"></a>Áttekintés
Ez az útmutató bemutatja, hogyan tooenable elérni az Azure Active Directory toohello developer portálon a felhasználók számára. Ez az útmutató azt is bemutatja, hogyan toomanage csoportok az Azure Active Directory-felhasználók külső tartalmazó csoportok hozzáadásával hello egy Azure Active Directory felhasználók.

> a jelen útmutató lépéseit toocomplete hello kell egy Azure Active Directory mely toocreate a kérelmet.
> 
> 

## <a name="how-tooauthorize-developer-accounts-using-azure-active-directory"></a>Hogyan tooauthorize fejlesztői fiókok Azure Active Directoryban
tooget elindítani, kattintson a **Publisher portal** a hello Azure-portálon az API Management szolgáltatás. Ezzel megnyitná toohello API Management publisher portálon.

![Közzétevő portál][api-management-management-console]

> Ha még nem hozott létre az API Management szolgáltatáspéldány, lásd: [hozzon létre egy API-kezelés szolgáltatás példányt] [ Create an API Management service instance] a hello [Ismerkedés az Azure API Management] [ Get started with Azure API Management] oktatóanyag.
> 
> 

Kattintson a **biztonsági** a hello **API Management** elemre, majd hello bal oldali menü **külső identitások**.

![Külső identitások][api-management-security-external-identities]

Kattintson a **az Azure Active Directory**. Jegyezze fel a hello **átirányítási URL-cím** és a klasszikus Azure portál hello Azure Active Directory tooyour keresztül.

![Külső identitások][api-management-security-aad-new]

Kattintson a hello **Hozzáadás** toocreate egy új Azure Active Directory-alkalmazás gombra, és válassza a **a szerveztem által fejlesztett alkalmazás hozzáadása**.

![Új Azure Active Directory-alkalmazás hozzáadása][api-management-new-aad-application-menu]

Adjon meg egy nevet hello alkalmazás, jelölje be **webes alkalmazáshoz és/vagy webes API**, majd hello Tovább gombra.

![Új Azure Active Directory-alkalmazás][api-management-new-aad-application-1]

A **bejelentkezési URL-cím**, adja meg a hello bejelentkezési URL-CÍMÉT a fejlesztői portálján. Ebben a példában hello **bejelentkezési URL-cím** van `https://aad03.portal.current.int-azure-api.net/signin`. 

A hello **azonosító URL-címet**, adja meg vagy hello alapértelmezett vagy egyéni tartomány hello Azure Active Directoryban, és egy egyedi karakterlánc tooit hozzáfűzése. Ebben a példában az alapértelmezett tartomány hello **https://contoso5api.onmicrosoft.com** hello utótagja használt **/api** megadott.

![Új Azure Active Directory-alkalmazás tulajdonságai][api-management-new-aad-application-2]

Hello ellenőrzése gombra toosave kattintson hello alkalmazás létrehozása és toohello kapcsoló **konfigurálása** tooconfigure hello új alkalmazás lapon.

![Új Azure Active Directory-alkalmazás létrehozása][api-management-new-aad-app-created]

Ha több aktív Azure-könyvtárak fog használni az alkalmazás toobe, kattintson a **Igen** a **alkalmazás több-bérlős**. hello alapértelmezett érték a **nem**.

![Alkalmazás több-bérlős][api-management-aad-app-multi-tenant]

Másolás hello **átirányítási URL-cím** a hello **Azure Active Directory** hello szakasza **külső identitások** hello publisher portálon lapra, majd illessze be hello **Válasz URL-CÍMEN** szövegmezőben. 

![Válasz URL-címe][api-management-aad-reply-url]

Görgessen toohello alsó részén hello konfigurálása lapon válassza hello **Alkalmazásengedélyek** legördülő, és ellenőrizze **címtáradatok olvasása**.

![Alkalmazásengedélyek][api-management-aad-app-permissions]

Jelölje be hello **delegált engedélyek** legördülő menüben, és ellenőrizze, **bejelentkezéssel, és olvassa el a felhasználói profilokon**.

![Delegált engedélyek][api-management-aad-delegated-permissions]

> További információ az alkalmazás és a delegált jogosultságokkal sikeresen telepítették: [elérése hello Graph API][Accessing hello Graph API].
> 
> 

Másolás hello **ügyfél-azonosító** toohello vágólapra.

![Ügyfél-azonosító][api-management-aad-app-client-id]

Váltson vissza toohello publisher portálon, és illessze be a hello **ügyfél-azonosító** hello Azure Active Directory-alkalmazás konfigurációja átmásolva.

![Ügyfél-azonosító][api-management-client-id]

Váltson vissza toohello Azure Active Directory konfigurációját, majd kattintson a hello **válassza ki a duration** hello legördülő **kulcsok** szakaszt, és adjon meg egy időközt. Ebben a példában **1 év** szolgál.

![Kulcs][api-management-aad-key-before-save]

Kattintson a **mentése** toosave hello konfigurációs és megjelenítési hello kulcs. Hello kulcs toohello vágólapra másolása.

> Jegyezze fel ezt a kulcsot. Hello Azure Active Directory konfigurációs ablak bezárása után hello kulcs nem jeleníthető meg újra.
> 
> 

![Kulcs][api-management-aad-key-after-save]

Kapcsoló hátsó toohello publisher portál és a Beillesztés hello kulcs be hello **Ügyfélkulcs** szövegmezőben.

![Ügyfélkulcs][api-management-client-secret]

**Bérlők engedélyezett** határozza meg, mely könyvtárak rendelkezik hozzáférési toohello API-k hello API Management service-példány. Az Azure Active Directory-példányok toowhich szeretne hozzáférni toogrant hello hello tartományok megadása. Elkülönítheti a több tartomány ágyazódjanak, szóközzel vagy vesszővel válassza el egymástól.

![Engedélyezett bérlők][api-management-client-allowed-tenants]


Amikor hello szükségeskonfiguráció-konfigurációt ad meg, kattintson a **mentése**.

![Mentés][api-management-client-allowed-tenants-save]

Miután hello módosítások mentésekor hello hello felhasználók megadott Azure Active Directory bejelentkezés toohello fejlesztői portálján hello utasításait követve [toohello fejlesztői portálján egy Azure Active Directory-fiókot használ-e jelentkezni] [Log in toohello Developer portal using an Azure Active Directory account].

Több tartomány is megadhatók hello **engedélyezett bérlők** szakasz. Mielőtt bármely felhasználó is jelentkezik be egy tartományból hello eredeti ahol hello alkalmazás lett regisztrálva, hello ugyanaz a tartományi globális rendszergazdájának kell engedélyt hello alkalmazás tooaccess címtáradatok. toogrant engedély hello globális rendszergazda léphetik túl`https://<URL of your developer portal>/aadadminconsent` (például https://contoso.portal.azure-api.net/aadadminconsent), hello toogive hozzáférés tooand kívánják Active Directory-bérlő hello tartománynevet írja be a Küldés gombra. Hello alábbi példa, a globális rendszergazdájának `miaoaad.onmicrosoft.com` próbál toogive engedély toothis adott fejlesztői portálján. 

![Engedélyek][api-management-aad-consent]

Hello következő képernyőn hello globális rendszergazdája lesz felszólító tooconfirm hello engedélyt adjon. 

![Engedélyek][api-management-permissions-form]

> Ha nem globális rendszergazda kísérli meg egy globális rendszergazda által nyújtott toolog az engedélyek előtt, hello bejelentkezési kísérlet sikertelen lesz, és egy hiba történt a képernyő jelenik meg.
> 
> 

## <a name="how-tooadd-an-external-azure-active-directory-group"></a>Hogyan tooadd egy külső az Azure Active Directory csoport
Miután engedélyezte a hozzáférést a felhasználók számára egy Azure Active Directory, adhat hozzá az API Management toomore Active Directory-csoportok, könnyedén kezelhető szükséges hello termékekkel hello hello fejlesztők hello csoport hozzárendelését.

> egy külső Azure Active Directory-csoportot, hello Azure Active Directory tooconfigure először konfigurálni kell hello identitások lapon hello előző szakaszban hello eljárást követve. 
> 
> 

Külső Active Directory-csoportok hozzáadott hello **látható** , amelynek toogrant hozzáférési toohello csoport kívánja hello termék fülre. Kattintson a **termékek**, majd kattintson a kívánt termék hello hello nevét.

![Termék konfigurálása][api-management-configure-product]

Váltás toohello **látható** fülre, majd **csoportok hozzáadása az Azure Active Directory**.

![Csoportok hozzáadása][api-management-add-groups]

Jelölje be hello **Azure Active Directory-bérlő** hello legördülő listából, és majd hello típusnév hello kívánt csoport hello **csoportok** toobe hozzáadott szövegmezőben.

![Válassza ki a csoport][api-management-select-group]

A csoport neve megtalálható hello **csoportok** az Azure Active Directoryban, ahogy az alábbi példa hello listájában.

![Az Azure Active Directory-csoportok listája][api-management-aad-groups-list]

Kattintson a **Hozzáadás** toovalidate hello csoport nevét, majd adja hozzá hello csoportot. Ebben a példában hello **Contoso 5 fejlesztők** külső csoport hozzá van adva. 

![Csoport hozzáadva][api-management-aad-group-added]

Kattintson a **mentése** toosave hello új csoport kijelölése.

Egy Azure Active Directory csoport konfigurációja egy terméket, ha már be van jelölve, a hello elérhető toobe **látható** lapján más termékek hello hello API Management service-példányban.

tooreview és konfigurálni hello tulajdonságait külső csoportot azok hozzáadott, kattintson a hello hello csoport hello neve **csoportok** fülre.

![Csoportok kezelése][api-management-groups]

Itt szerkesztheti hello **neve** és hello **leírás** hello csoport.

![Csoport szerkesztése][api-management-edit-group]

Hello felhasználók konfigurált Azure Active Directory toohello fejlesztői portálján és nézet bejelentkezhet és előfizetés tooany csoportok amelyeknél látható a következő szakasz hello hello utasításait követve.

## <a name="how-toolog-in-toohello-developer-portal-using-an-azure-active-directory-account"></a>Hogyan toolog toohello Developer portálon az Azure Active Directory-fiókkal
hello fejlesztői portálra hello korábbi szakaszokban, konfigurált Azure Active Directory-fiókkal történő toolog nyisson meg egy új böngészőablakot, hello segítségével **bejelentkezési URL-cím** hello Active Directory-alkalmazás konfigurációját, majd kattintson a **Az azure Active Directory**.

![Fejlesztői portálján][api-management-dev-portal-signin]

Adja meg a hello hitelesítő adatokat az egyik hello felhasználók az Azure Active Directoryban, majd kattintson **bejelentkezés**.

![Bejelentkezés][api-management-aad-signin]

Kérheti a regisztrációs űrlap Ha bármilyen további információkra szükség. Hello regisztrációs űrlap kitöltése és kattintson a **regisztráljon**.

![Regisztráció][api-management-complete-registration]

A felhasználók most már be legyen jelentkezve az API Management szolgáltatáspéldány hello fejlesztői portálján.

![Regisztráció kész][api-management-registration-complete]

[api-management-management-console]: ./media/api-management-howto-aad/api-management-management-console.png
[api-management-security-external-identities]: ./media/api-management-howto-aad/api-management-security-external-identities.png
[api-management-security-aad-new]: ./media/api-management-howto-aad/api-management-security-aad-new.png
[api-management-new-aad-application-menu]: ./media/api-management-howto-aad/api-management-new-aad-application-menu.png
[api-management-new-aad-application-1]: ./media/api-management-howto-aad/api-management-new-aad-application-1.png
[api-management-new-aad-application-2]: ./media/api-management-howto-aad/api-management-new-aad-application-2.png
[api-management-new-aad-app-created]: ./media/api-management-howto-aad/api-management-new-aad-app-created.png
[api-management-aad-app-permissions]: ./media/api-management-howto-aad/api-management-aad-app-permissions.png
[api-management-aad-app-client-id]: ./media/api-management-howto-aad/api-management-aad-app-client-id.png
[api-management-client-id]: ./media/api-management-howto-aad/api-management-client-id.png
[api-management-aad-key-before-save]: ./media/api-management-howto-aad/api-management-aad-key-before-save.png
[api-management-aad-key-after-save]: ./media/api-management-howto-aad/api-management-aad-key-after-save.png
[api-management-client-secret]: ./media/api-management-howto-aad/api-management-client-secret.png
[api-management-client-allowed-tenants]: ./media/api-management-howto-aad/api-management-client-allowed-tenants.png
[api-management-client-allowed-tenants-save]: ./media/api-management-howto-aad/api-management-client-allowed-tenants-save.png
[api-management-aad-delegated-permissions]: ./media/api-management-howto-aad/api-management-aad-delegated-permissions.png
[api-management-dev-portal-signin]: ./media/api-management-howto-aad/api-management-dev-portal-signin.png
[api-management-aad-signin]: ./media/api-management-howto-aad/api-management-aad-signin.png
[api-management-complete-registration]: ./media/api-management-howto-aad/api-management-complete-registration.png
[api-management-registration-complete]: ./media/api-management-howto-aad/api-management-registration-complete.png
[api-management-aad-app-multi-tenant]: ./media/api-management-howto-aad/api-management-aad-app-multi-tenant.png
[api-management-aad-reply-url]: ./media/api-management-howto-aad/api-management-aad-reply-url.png
[api-management-aad-consent]: ./media/api-management-howto-aad/api-management-aad-consent.png
[api-management-permissions-form]: ./media/api-management-howto-aad/api-management-permissions-form.png
[api-management-configure-product]: ./media/api-management-howto-aad/api-management-configure-product.png
[api-management-add-groups]: ./media/api-management-howto-aad/api-management-add-groups.png
[api-management-select-group]: ./media/api-management-howto-aad/api-management-select-group.png
[api-management-aad-groups-list]: ./media/api-management-howto-aad/api-management-aad-groups-list.png
[api-management-aad-group-added]: ./media/api-management-howto-aad/api-management-aad-group-added.png
[api-management-groups]: ./media/api-management-howto-aad/api-management-groups.png
[api-management-edit-group]: ./media/api-management-howto-aad/api-management-edit-group.png

[How tooadd operations tooan API]: api-management-howto-add-operations.md
[How tooadd and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: api-management-monitoring.md
[Add APIs tooa product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Get started with Azure API Management]: api-management-get-started.md
[API Management policy reference]: api-management-policy-reference.md
[Caching policies]: api-management-policy-reference.md#caching-policies
[Create an API Management service instance]: api-management-get-started.md#create-service-instance

[http://oauth.net/2/]: http://oauth.net/2/
[WebApp-GraphAPI-DotNet]: https://github.com/AzureADSamples/WebApp-GraphAPI-DotNet
[Accessing hello Graph API]: http://msdn.microsoft.com/library/azure/dn132599.aspx#BKMK_Graph

[Prerequisites]: #prerequisites
[Configure an OAuth 2.0 authorization server in API Management]: #step1
[Configure an API toouse OAuth 2.0 user authorization]: #step2
[Test hello OAuth 2.0 user authorization in hello Developer Portal]: #step3
[Next steps]: #next-steps

[Log in toohello Developer portal using an Azure Active Directory account]: #Log-in-to-the-Developer-portal-using-an-Azure-Active-Directory-account

