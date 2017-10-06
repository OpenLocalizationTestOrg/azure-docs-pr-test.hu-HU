---
title: aaaAzure AD B2C |} Microsoft Docs
description: "hello típusú alkalmazásokat hozhat létre az Azure Active Directory B2C hello."
services: active-directory-b2c
documentationcenter: 
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: bb9d4abe-0db7-4bd9-b0c4-2f43b2c9cf33
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 12/06/2016
ms.author: dastrock
ms.openlocfilehash: 7dd3dac781fb7e1553dd0f2d112b1956489a7dfd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-types-of-applications"></a>Azure Active Directory B2C: Alkalmazások típusai
Az Azure Active Directory (Azure AD) B2C számos különböző modern alkalmazásarchitektúrához használható hitelesítést tartalmaz. Az összes hello iparági szabványos protokollok alapulnak [OAuth 2.0](active-directory-b2c-reference-protocols.md) vagy [OpenID Connect](active-directory-b2c-reference-protocols.md). Ez a dokumentum röviden ismerteti hello alkalmazástípust hozhat létre, hello nyelvi vagy platform független inkább. Azt is segít megérteni a magas szintű forgatókönyvek hello előtt [alkalmazások készítéséhez](active-directory-b2c-overview.md#get-started).

## <a name="hello-basics"></a>hello alapjai
Minden Azure AD B2C használó alkalmazást regisztrálni kell a [B2C-címtár](active-directory-b2c-get-started.md) keresztül hello [Azure Portal](https://portal.azure.com/). hello az alkalmazásregisztrációs művelet során gyűjt, és hozzárendeli a néhány értékek tooyour alkalmazást:

* **Application ID** (Alkalmazásazonosító), amely egyedileg azonosítja az alkalmazást.
* A **átirányítási URI-** használt toodirect válaszok hátsó tooyour alkalmazás, amely lehet.
* Más, az adott feladathoz tartozó értékek. További részletekért megtudhatja, hogyan túl[alkalmazások regisztrálásának folyamatával](active-directory-b2c-app-registration.md).

Hello alkalmazás regisztrálása után kommunikál az Azure AD küldött kérelmek toohello az Azure AD v2.0-végponttal:

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize
https://login.microsoftonline.com/common/oauth2/v2.0/token
```

Minden AD B2C tooAzure küldött kérelmek megadja egy **házirend**. A szabályzatok vezérlik az Azure AD hello működését. A végpontok toocreate felhasználói élményt nagy mértékben testre szabható készlete is használható. Gyakran használt szabályzatok például a következők: regisztráció, bejelentkezés, profilmódosítás és így tovább. Ha nincs tisztában a szabályzatokkal, olvasson utána az Azure AD B2C hello [bővíthető szabályzat-keretrendszernek](active-directory-b2c-reference-policies.md) a folytatás előtt.

a v2.0-végpontra a hello minden alkalmazás interakcióba hasonló felső szintű mintát követi:

1. hello alkalmazás elirányítja hello felhasználói toohello v2.0 végpont tooexecute egy [házirend](active-directory-b2c-reference-policies.md).
2. hello felhasználó befejezi a hello házirend szerint toohello házirend-definíció.
3. hello alkalmazás valamilyen biztonsági jogkivonatot kap hello v2.0-végponttól.
4. hello alkalmazás hello biztonsági jogkivonat tooaccess védett adatokat vagy a védett erőforrás használ.
5. hello erőforrás kiszolgáló hello biztonsági jogkivonat tooverify, amelyek hozzáférést is érvényesíti.
6. hello alkalmazás rendszeres időközönként frissíti hello biztonsági jogkivonatot.

<!-- TODO: Need a page for libraries toolink too-->
A lépések némileg eltérőek lehetnek hello típusú alkalmazás típusától függően. Nyílt forráskódú kódtárakból is hello részletek meg.

## <a name="web-apps"></a>Webalkalmazások
A kiszolgálón futtatott és böngészőn keresztül elért webalkalmazások (ideértve a .NET- , a PHP-, a Java-, a Ruby-, a Python- és a Node.js-alapú alkalmazásokat) esetében az Azure AD B2C az összes felhasználói élmény esetében támogatja az [OpenID Connect](active-directory-b2c-reference-protocols.md) protokollt. Ide tartozik a bejelentkezés, a regisztráció és a profilkezelés is. Az OpenID Connect Azure AD B2C hello megvalósításában a webes alkalmazás hitelesítési kérelmek tooAzure AD kiállításával indít el a különböző felhasználói élményeket. hello hello kérelem eredménye egy `id_token`. Ez a biztonsági jogkivonat hello felhasználói identitás jelöli. Azt is megtudhatja hello felhasználói jogcímeket hello formájában:

```
// Partial raw id_token
eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6ImtyaU1QZG1Cd...

// Partial content of a decoded id_token
{
    "name": "John Smith",
    "email": "john.smith@gmail.com",
    "oid": "d9674823-dffc-4e3f-a6eb-62fe4bd48a58"
    ...
}
```

További tudnivalók hello típusú jogkivonatok és jogcímek elérhető tooan alkalmazás hello [B2C-jogkivonatok referenciájából](active-directory-b2c-reference-tokens.md).

A webalkalmazásban a [szabályzat](active-directory-b2c-reference-policies.md) minden egyes végrehajtása a következő magas szintű lépéseket járja végig:

![Webalkalmazás, sávok](./media/active-directory-b2c-apps/webapp.png)

Hello érvényesítése `id_token` az Azure AD-tól kapott nyilvános aláírókulcs segítségével van elegendő tooverify hello identitás hello felhasználó. Ezzel egyúttal beállít egy munkamenetcookie-t, amely a következő lapkérések használt tooidentify hello felhasználó.

toosee a folyamatot, próbálja ki valamelyik hello web app bejelentkezési mintakódjainak megtekintése a a [kezdeti lépéseket bemutató cikk](active-directory-b2c-overview.md#get-started).

Továbbá toofacilitating egyszerű bejelentkezés, a kiszolgáló webalkalmazás is szükség lehet egy háttér-webszolgáltatás tooaccess. Ebben az esetben hello webalkalmazást egy némileg eltérő hajthat végre [OpenID Connect-folyamat](active-directory-b2c-reference-oidc.md) és jogkivonatok szerezni a hitelesítési kódok és frissítési jogkivonatok. Ebben a forgatókönyvben hello következő írja le [webes API-k szakasz](#web-apis).

<!--, and in our [WebApp-WebAPI Getting started topic](active-directory-b2c-devquickstarts-web-api-dotnet.md).-->

## <a name="web-apis"></a>Webes API-k
Az Azure AD B2C toosecure webes szolgáltatások például az alkalmazás RESTful webes API segítségével. Webes API-k használatához OAuth 2.0 toosecure adataikat, bejövő HTTP-kérelmek jogkivonatok történő hitelesítéséhez. a webes API hello hívó hozzáfűz egy jogkivonatot a HTTP-kérések hello engedélyezési fejléc:

```
GET /api/items HTTP/1.1
Host: www.mywebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6...
Accept: application/json
...
```

hello webes API hello token tooverify hello API hívó identitása és tooextract információ hello hívó hello jogkivonatban kódolt jogcímek használható. További tudnivalók hello típusú jogkivonatok és jogcímek elérhető tooan alkalmazás hello [Azure AD B2C-jogkivonatok referenciájából](active-directory-b2c-reference-tokens.md).

> [!NOTE]
> Az Azure AD B2C jelenleg csak azokat a webes API-kat támogatja, amelyekhez a saját, jól ismert ügyfeleikkel lehet hozzáférni. Az elkészült alkalmazás tartalmazhat például egy iOS-alkalmazást, egy Android-alkalmazást, valamint egy háttérben futó webes API-t. Ez az architektúra már most is teljes támogatást élvez. Egy másik iOS-alkalmazás, például egy partnerügyfél tooaccess hello azonos webes API jelenleg nem támogatott. A teljes alkalmazás hello összetevőket kell osztania egy egyetlen alkalmazáscsoportot.
>
>

A webes API számos különböző típusú ügyféltől (például webalkalmazásoktól, asztali és mobilalkalmazásoktól, egylapos alkalmazásoktól, kiszolgálóoldali démonoktól vagy más webes API-któl) képes jogkivonatokat fogadni. Íme egy példa egy webes API-t meghívó webalkalmazás teljes folyamatára hello:

![Webalkalmazás, webes API sávok](./media/active-directory-b2c-apps/webapi.png)

További információk a hitelesítési kódok, frissítési jogkivonatokról, valamint a jogkivonatok, lekérésének lépéseiről hello toolearn olvassa el hello [OAuth 2.0 protokollt](active-directory-b2c-reference-oauth-code.md).

Hogyan toosecure egy webes API-t az Azure AD B2C hello kivételét használatával webes API-kra vonatkozó toolearn a [kezdeti lépéseket bemutató cikk](active-directory-b2c-overview.md#get-started).

## <a name="mobile-and-native-apps"></a>Mobil- és natív alkalmazások
Hordozható és asztali alkalmazások, például az eszközökre telepített alkalmazások gyakran kell tooaccess háttér-szolgáltatásaihoz vagy webes API-k felhasználók nevében. Egyedi identitáskezelési felügyeleti feladatait tooyour natív alkalmazások hozzáadása és a háttér-szolgáltatásaihoz biztonságosan hívja az Azure AD B2C és hello segítségével [OAuth 2.0 hitelesítésikód-folyamata](active-directory-b2c-reference-oauth-code.md).  

Ez a folyamat a hello alkalmazás végrehajtja a [házirendek](active-directory-b2c-reference-policies.md) és kap egy `authorization_code` Azure ad-felhasználó hello hello házirend befejeződése után. Hello `authorization_code` jelöli hello alkalmazás engedély toocall háttér-szolgáltatásaihoz hello aktuálisan bejelentkezett felhasználó nevében. hello app majd továbbíthatja az hello `authorization_code` a hello háttérben egy `id_token` és egy `refresh_token`.  hello app használhatók hello `id_token` tooauthenticate tooa háttér-webes API-t a HTTP-kérelmekre. Hello is használni tudja `refresh_token` tooget új `id_token` amikor a régi lejárna.

> [!NOTE]
> Az Azure AD B2C jelenleg csak jogkivonatokat használt tooaccess az alkalmazás saját webes szolgáltatás. Az elkészült alkalmazás tartalmazhat például egy iOS-alkalmazást, egy Android-alkalmazást, valamint egy háttérben futó webes API-t. Ez az architektúra már most is teljes támogatást élvez. OAuth 2.0 hozzáférési jogkivonatok segítségével képes a partneri webes API az iOS app tooaccess így jelenleg nem támogatott. A teljes alkalmazás hello összetevőket kell osztania egy egyetlen alkalmazáscsoportot.
>
>

![Natív alkalmazás, sávok](./media/active-directory-b2c-apps/native.png)

## <a name="current-limitations"></a>Aktuális korlátozások
Az Azure AD B2C jelenleg nem támogatja a következő típusú alkalmazásokat hello, de ezek hello terv is. 

### <a name="daemonsserver-side-apps"></a>Démonok/kiszolgálóoldali alkalmazások
Az alkalmazások hosszú futású folyamatokat tartalmazó, vagy a felhasználó hello jelenléte nélkül is szükség van egy tooaccess védett erőforrások, például webes API-k. Ezek az alkalmazások hitelesítheti és a jogkivonatok lekérésére, hello app identitásuk (azaz nem a felhasználó delegált identitása) használatával és hello OAuth 2.0 ügyfél-hitelesítő adatok folyamata segítségével.

Az Azure AD B2C jelenleg nem támogatja a folyamatot. Ezek az alkalmazások csak akkor képesek a jogkivonatok lekérésére, ha már végbement egy interaktív felhasználói folyamat.

### <a name="web-api-chains-on-behalf-of-flow"></a>Webes API-láncok (meghatalmazásos folyamat)
Számos architektúra tartalmaz egy webes API-t, amelyet a toocall egy másik alsóbb rétegbeli webes API-t, ha mind az Azure AD B2C védi. Ez gyakori a webes API-háttérrel rendelkező natív ügyfelek esetében. Ez aztán meghív egy Microsoft Online Services szolgáltatással, például az Azure AD Graph API hello.

Ez a láncolatba fűzött webes API-forgatókönyv hello OAuth 2.0 JWT tulajdonosi hitelesítő adatok megadásával, más néven hello a-meghatalmazásos folyamat használatával támogatható.  Azonban hello a-meghatalmazásos folyamat jelenleg nincs megvalósítva az Azure AD B2C hello.
