---
title: "az OAuth 2.0 verziót használja az Azure API Management aaaAuthorize fejlesztői fiókok |} Microsoft Docs"
description: "Megtudhatja, hogyan tooauthorize felhasználók OAuth 2.0 API Management használata."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 78c48247-64f0-4708-b2d0-98b61a821283
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: 934901dd6df399470a3257bf7a3a9b9fb5f40d5e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooauthorize-developer-accounts-using-oauth-20-in-azure-api-management"></a>Hogyan tooauthorize fejlesztői fiókok OAuth 2.0, az Azure API Management
Számos API támogatja [OAuth 2.0](http://oauth.net/2/) toosecure hello API, és győződjön meg arról, hogy csak érvényes felhasználók hozzáférhetnek, és csak eléréséhez erőforrások toowhich azok még jogosult. A sorrend toouse Azure API Management meg interaktív fejlesztői konzolján ilyen API-khoz, hello szolgáltatás lehetővé teszi tooconfigure a szolgáltatás példány toowork az OAuth 2.0 API engedélyezve van.

## <a name="prerequisites"></a>Előfeltételek
Ez az útmutató bemutatja, miként tooconfigure az API Management szolgáltatás példány toouse OAuth 2.0-engedélyezés fejlesztői fiókok számára, azonban nem mutatja be tooconfigure OAuth 2.0-s szolgáltató. Minden szolgáltató eltér, bár hello lépések hasonlóak, és az API Management szolgáltatáspéldány OAuth 2.0 konfigurálásához használt szükséges hello adatra OAuth 2.0 hello konfigurációs hello azonos. Ez a témakör bemutatja az Azure Active Directoryt használja az OAuth 2.0-s szolgáltató példák.

> [!NOTE]
> Az Azure Active Directoryval OAuth 2.0 konfigurálásáról további információkért lásd: hello [WebApp-GraphAPI-DotNet] [ WebApp-GraphAPI-DotNet] minta.
> 
> 

## <a name="step1"></a>OAuth 2.0 hitelesítési kiszolgáló beállítása az API Management
tooget elindítani, kattintson a **Publisher portal** a hello Azure portál, az API Management szolgáltatás.

![Közzétevő portál][api-management-management-console]

> [!NOTE]
> Ha még nem hozott létre az API Management szolgáltatáspéldány, lásd: [hozzon létre egy API-kezelés szolgáltatás példányt] [ Create an API Management service instance] a hello [Ismerkedés az Azure API Management] [ Get started with Azure API Management] oktatóanyag.
> 
> 

Kattintson a **biztonsági** a hello **API Management** menü hello balra, kattintson a **OAuth 2.0**, és kattintson a **engedélyezési kiszolgáló hozzáadása**.

![OAuth 2.0][api-management-oauth2]

Miután rákattintott **engedélyezési kiszolgáló hozzáadása**, hello új engedélyezési server képernyő jelenik meg.

![Új kiszolgáló][api-management-oauth2-server-1]

Adjon meg egy nevet és leírást nem kötelező hello **neve** és **leírás** mezőket. 

> [!NOTE]
> A mezők kitöltése használt tooidentify hello OAuth 2.0 hitelesítési kiszolgáló hello aktuális API Management szolgáltatáspéldány belül, és azok értékeit nem hello OAuth 2.0 kiszolgálóról származnak.
> 
> 

Adja meg a hello **ügyfél regisztrációs URL-címe**. Ez a lap, ahol a felhasználók létrehozása és azok a fiókok kezelése és a használt hello OAuth 2.0-s szolgáltató függ. Hello **ügyfél regisztrációs URL-címe** toohello lapon, hogy a felhasználók pontok toocreate használhatja és a saját felhasználói konfigurálása a felhasználói fiókok kezelését támogató OAuth 2.0-s szolgáltatók. Egyes szervezetek konfigurálásához, vagy nem használja ezt a funkciót, még akkor is, ha hello OAuth 2.0-s szolgáltató támogatja. Az OAuth 2.0-s szolgáltató nem rendelkezik konfigurált fiókok felhasználói kezelését, ha meg egy helyőrzőt URL-címet itt hello vállalata, és egy URL-címe például például `https://placeholder.contoso.com`.

hello hello űrlap következő szakaszában található hello **engedélyezési kódot adjon típusok**, **engedélyezési végpont URL-címet**, és **engedélyezési metódus** beállításait.

![Új kiszolgáló][api-management-oauth2-server-2]

Adja meg a hello **engedélyezési kódot adjon típusok** szükséges hello típusok ellenőrzésével. **Engedélyezési kód** alapértelmezés szerint van megadva.

Adja meg a hello **engedélyezési végpont URL-címet**. Az Azure Active Directory, az URL-cím lesz hasonló toohello a következő URL-cím, ahol `<client_id>` hello ügyfél-azonosító, amely azonosítja az alkalmazás OAuth 2.0 toohello server helyére.

`https://login.microsoftonline.com/<client_id>/oauth2/authorize`

Hello **engedélyezési metódus** határozza meg, hogyan hello engedélyezési kérelmet küldött toohello OAuth 2.0-kiszolgáló. Alapértelmezés szerint **beolvasása** van kiválasztva.

hello a következő szakaszban van, ahol hello **végponti URL-cím Token**, **ügyfél-hitelesítési módszerek**, **küldése metódus hozzáférési jogkivonat**, és **hatóköralapértelmezett** vannak megadva.

![Új kiszolgáló][api-management-oauth2-server-3]

Hello Azure Active Directory OAuth 2.0-kiszolgáló esetén **végponti URL-cím a Token** lesz hello a következő formátumban, ahol `<APPID>` hello formátuma a `yourapp.onmicrosoft.com`.

`https://login.microsoftonline.com/<APPID>/oauth2/token`

Alapértelmezés szerint hello **ügyfél-hitelesítési módszerek** van **alapvető**, és **küldése metódus hozzáférési jogkivonat** van **Authorization fejlécet**. Ezek az értékek vannak konfigurálva a ebben a szakaszban hello képernyő, valamint hello **hatókör alapértelmezett**.

Hello **ügyfél hitelesítő adatait** szakasz hello **ügyfél-azonosító** és **ügyfélkulcs**, amely akkor kapja meg az OAuth 2.0 létrehozása és a konfigurációs folyamat hello során a kiszolgáló. Egyszer hello **ügyfél-azonosító** és **ügyfélkulcs** vannak megadva, hello **redirect_uri** a hello **engedélyezési kód** jön létre. Ezt az URI használt tooconfigure hello válasz URL-CÍMEN az OAuth 2.0-kiszolgálói konfigurációban.

![Új kiszolgáló][api-management-oauth2-server-4]

Ha **engedélyezési kódot adjon típusok** értéke túl**erőforrás tulajdonosi jelszó**, hello **erőforrás tulajdonosa jelszavas hitelesítő adatokat** szakaszban használt toospecify azokat a hitelesítő adatok; Ellenkező esetben üresen azt.

![Új kiszolgáló][api-management-oauth2-server-5]

Hello űrlap befejeztével kattintson **mentése** toosave hello API Management OAuth 2.0 hitelesítési kiszolgáló konfigurációját. Után hello kiszolgálókonfiguráció mentésekor állíthatja be API-k toouse ebben a konfigurációban hello a következő szakaszban látható.

## <a name="step2"></a>Egy API toouse OAuth 2.0 felhasználói hitelesítés konfigurálása
Kattintson a **API-k** a hello **API Management** hello menüjének balra kattintson a kívánt hello API hello neve, kattintson **biztonsági**, és a majd hello jelölőnégyzetet **OAuth 2.0**.

![Felhasználó engedélyezése][api-management-user-authorization]

Jelölje be hello szükséges **engedélyezési server** hello legördülő listából, és kattintson a **mentése**.

![Felhasználó engedélyezése][api-management-user-authorization-save]

## <a name="step3"></a>Hello fejlesztői portálján hello OAuth 2.0 felhasználói hitelesítés tesztelése
Miután beállította az OAuth 2.0 hitelesítési kiszolgáló és az API toouse konfigurálva a kiszolgálón, tesztelheti toohello fejlesztői portálján is, és az API felület meghívásakor.  Kattintson a **fejlesztői portálján** hello jobb felső menüjében található.

![Fejlesztői portál][api-management-developer-portal-menu]

Kattintson a **API-k** hello felső menüre, majd válassza a **Echo API**.

![Echo API][api-management-apis-echo-api]

> [!NOTE]
> Ha csak egy API konfigurálva van, vagy látható tooyour fiókot, majd kattintson az API-k viszi közvetlenül toohello műveletek, hogy az API-hoz.
> 
> 

SELECT hello **erőforrás beolvasása** műveletet, kattintson a **nyissa meg a konzolt**, majd válassza ki **engedélyezési kód** a hello legördülő.

![Konzol megnyitása][api-management-open-console]

Ha **engedélyezési kód** van jelölve, egy előugró ablak jelenik meg, amely hello bejelentkezési űrlap hello OAuth 2.0-s szolgáltató. Ebben a példában hello bejelentkezési űrlap Azure Active Directory által biztosított.

> [!NOTE]
> Ha az előugró ablakok le van tiltva, akkor kéri tooenable hello böngésző őket. Miután engedélyezte őket, válassza ki a **engedélyezési kód** újra és hello bejelentkezési képernyő jelenik meg.
> 
> 

![Bejelentkezés][api-management-oauth2-signin]

Miután bejelentkezett, hello **kérelem fejlécei** kerülnek egy `Authorization : Bearer` , amely engedélyezi a hello kérelem fejléce.

![Kérelem fejléc jogkivonat][api-management-request-header-token]

Ezen a ponton konfigurálja a fennmaradó paraméterek hello hello szükséges értékeket, és hello kérelem küldése. 

## <a name="next-steps"></a>Következő lépések
OAuth 2.0-s és API-kezelés használatával kapcsolatos további információkért tekintse meg a video- és a hozzá tartozó hello következőt [cikk](api-management-howto-protect-backend-with-aad.md).

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Protecting-Web-API-Backend-with-Azure-Active-Directory-and-API-Management/player]
> 
> 

[api-management-management-console]: ./media/api-management-howto-oauth2/api-management-management-console.png
[api-management-oauth2]: ./media/api-management-howto-oauth2/api-management-oauth2.png
[api-management-user-authorization]: ./media/api-management-howto-oauth2/api-management-user-authorization.png
[api-management-user-authorization-save]: ./media/api-management-howto-oauth2/api-management-user-authorization-save.png
[api-management-oauth2-signin]: ./media/api-management-howto-oauth2/api-management-oauth2-signin.png
[api-management-request-header-token]: ./media/api-management-howto-oauth2/api-management-request-header-token.png
[api-management-developer-portal-menu]: ./media/api-management-howto-oauth2/api-management-developer-portal-menu.png
[api-management-open-console]: ./media/api-management-howto-oauth2/api-management-open-console.png
[api-management-oauth2-server-1]: ./media/api-management-howto-oauth2/api-management-oauth2-server-1.png
[api-management-oauth2-server-2]: ./media/api-management-howto-oauth2/api-management-oauth2-server-2.png
[api-management-oauth2-server-3]: ./media/api-management-howto-oauth2/api-management-oauth2-server-3.png
[api-management-oauth2-server-4]: ./media/api-management-howto-oauth2/api-management-oauth2-server-4.png
[api-management-oauth2-server-5]: ./media/api-management-howto-oauth2/api-management-oauth2-server-5.png
[api-management-apis-echo-api]: ./media/api-management-howto-oauth2/api-management-apis-echo-api.png


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

[Prerequisites]: #prerequisites
[Configure an OAuth 2.0 authorization server in API Management]: #step1
[Configure an API toouse OAuth 2.0 user authorization]: #step2
[Test hello OAuth 2.0 user authorization in hello Developer Portal]: #step3
[Next steps]: #next-steps

