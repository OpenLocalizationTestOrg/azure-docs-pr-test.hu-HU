---
title: "hello Azure Active Directory v2.0-végponttól aaaApp típusokat |} Microsoft Docs"
description: "alkalmazások és a v2.0-végponttól hello Azure Active Directory által támogatott forgatókönyveket hello típusú."
services: active-directory
documentationcenter: 
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: 494a06b8-0f9b-44e1-a7a2-d728cf2077ae
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/01/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: db95c88d6865abac8ce80378ccd6b63cb38e0a01
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="app-types-for-hello-azure-active-directory-v20-endpoint"></a>Az Azure Active Directory v2.0-végponttól hello alkalmazástípusok
hello Azure Active Directory (Azure AD) v2.0-végponttól hitelesítést is támogatja a modern alkalmazás-architektúrák esetén az összes szabványos protokollokat számos [OAuth 2.0-s vagy az OpenID Connect](active-directory-v2-protocols.md). Ez a cikk ismerteti a hello típusú alkalmazásokat, amelyek az Azure AD v2.0, függetlenül a választott nyelv vagy platform használatával hozhat létre. hello a cikkben szereplő információkat az összetettebb feladatok előtt tisztában tervezett toohelp [hello kóddal munka megkezdéséhez](active-directory-appmodel-v2-overview.md#getting-started).

> [!NOTE]
> hello v2.0-végpontra nem támogatja, minden Azure Active Directory forgatókönyvek és funkciók. toodetermine kell használnia a hello v2.0-végpontra, hogy olvassa el [v2.0 korlátozások](active-directory-v2-limitations.md).
> 
> 

## <a name="hello-basics"></a>hello alapjai
Regisztrálnia kell az egyes alkalmazások hello hello v2.0-végponttól használó [Microsoft alkalmazásregisztrációs portálra](https://apps.dev.microsoft.com). hello az alkalmazásregisztrációs művelet során gyűjt, és hozzárendeli ezeket az értékeket az alkalmazás:

* Egy **Alkalmazásazonosító** , amely egyedileg azonosítja az alkalmazást
* A **átirányítási URI-** használható toodirect válaszok hátsó tooyour alkalmazás
* Néhány más forgatókönyvekre jellemző értékek

További információkért megtudhatja, hogyan túl[alkalmazások regisztrálásának folyamatával](active-directory-v2-app-registration.md).

Hello alkalmazás regisztrálása után hello app kommunikál az Azure AD küldött kérelmek toohello az Azure AD v2.0-végponttól. Azt adja meg, nyílt forráskódú keretrendszerek és tárak, amelyek kezelik ezeket a kérelmeket hello részleteit. Akkor is hello beállítás tooimplement hello hitelesítési logikát saját kérések toothese végpontok létrehozásával:

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize
https://login.microsoftonline.com/common/oauth2/v2.0/token
```
<!-- TODO: Need a page for libraries toolink too-->

## <a name="web-apps"></a>Webalkalmazások
A web Apps (.NET, PHP, Java, Ruby, Python, csomópont), amely hello felhasználói hozzáférések böngészőn keresztül, használhatja a [OpenID Connect](active-directory-v2-protocols.md) a felhasználói bejelentkezés. Az OpenID Connect hello webalkalmazás kap egy azonosító jogkivonatot. Egy azonosító lexikális elem egy biztonsági jogkivonatot, ellenőrzi a hello felhasználó identitását, és hello felhasználói hello jogcímek formájában információkat nyújt:

```
// Partial raw ID token
eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6ImtyaU1QZG1Cd...

// Partial content of a decoded ID token
{
    "name": "John Smith",
    "email": "john.smith@gmail.com",
    "oid": "d9674823-dffc-4e3f-a6eb-62fe4bd48a58"
    ...
}
```

Minden hello jogkivonatok és jogcímek különböző típusairól, amelyek a rendelkezésre álló tooan alkalmazás hello megismerheti [v2.0 jogkivonatok hivatkozás](active-directory-v2-tokens.md).

A kiszolgáló webalkalmazásokban hello bejelentkezés hitelesítési folyamata magas szintű lépéseket hajtja végre:

![Webes alkalmazás hitelesítési folyamat](../../media/active-directory-v2-flows/convergence_scenarios_webapp.png)

Hello felhasználói identitás biztosítható, ha a hello azonosító token érvényesítés a v2.0-végponttól hello-tól kapott nyilvános aláírókulcs. Egy munkamenetcookie-t van beállítva, amely a következő lapkérések használt tooidentify hello felhasználó lehet.

a 2.0-s verziója minták ebben a forgatókönyvben a művelet, próbálkozzon egy webes alkalmazás bejelentkezési kód hello toosee [bevezetés](active-directory-appmodel-v2-overview.md#getting-started) szakasz.

Ezenkívül toosimple bejelentkezés, a webalkalmazás-kiszolgáló kell tooaccess egy másik webes szolgáltatás, például egy REST API-t. Ebben az esetben hello kiszolgáló a webalkalmazás kapcsolatba lép egy kombinált OpenID Connectet és az OAuth 2.0 folyamatában, hello segítségével [OAuth 2.0 hitelesítésikód-folyamata](active-directory-v2-protocols.md). Ebben a forgatókönyvben kapcsolatos további információkért olvassa el [Ismerkedés a webalkalmazások és webes API-k](active-directory-v2-devquickstarts-webapp-webapi-dotnet.md).

## <a name="web-apis"></a>Webes API-k
Hello v2.0 végpont toosecure webes szolgáltatások, például az alkalmazás RESTful webes API segítségével. Azonosító-jogkivonatokat és a munkamenet cookie-k helyett a egy webes API-t az OAuth 2.0 hozzáférési jogkivonat toosecure használ, az adatok és tooauthenticate bejövő kérelmeket. hello hívó egy webes API hozzáfűz egy jogkivonatot a HTTP-kérések, ilyen hello authorization fejlécet:

```
GET /api/items HTTP/1.1
Host: www.mywebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6...
Accept: application/json
...
```

Webes API hello hello access token tooverify hello API hívójának identitását és tooextract információt hello hívó hello hozzáférési jogkivonat kódolt jogcímek használja. toolearn kapcsolatos összes hello jogkivonatok és jogcímek különböző típusairól, amelyek a rendelkezésre álló tooan app, lásd: hello [v2.0 jogkivonatok hivatkozás](active-directory-v2-tokens.md).

Egy webes API-t ad felhasználók hello power tooopt, vagy tilthatják le az adott funkció vagy adatok engedélyek, más néven jelentkezik, mintha [hatókörök](active-directory-v2-scopes.md). A hívó alkalmazás tooacquire engedély tooa hatókör, hello felhasználónak bele kell egyeznie toohello hatókör a folyamat során. hello v2.0-végponttól hello felhasználói kér engedélyt, és majd engedélyek rögzít minden hozzáférési jogkivonatok adott hello Web API kap. Webes API hello érvényesíti hello hozzáférési jogkivonatok egyes hívás fogadása és engedélyezési ellenőrzi.

Egy webes API-alkalmazás, illetve a kiszolgáló webalkalmazások, asztali és mobile apps szolgáltatásban, egyoldalas alkalmazások, kiszolgálóoldali démonok, és akár egyéb webes API-k minden típusú hozzáférési jogkivonatok fogadhat. a webes API hello magas szintű folyamata így néz ki:

![Webes API-hitelesítési folyamat](../../media/active-directory-v2-flows/convergence_scenarios_webapi.png)

Hogyan toosecure egy webes API OAuth2 hozzáférési jogkivonatok hello Web API-kódban kivételt a mintákkal toolearn a [bevezetés](active-directory-appmodel-v2-overview.md#getting-started) szakasz.

Sok esetben a webes API-k toomake kimenő kérelmek tooother után webes API-khoz, Azure Active Directory által biztosított is kell.  toodo tehát webes API-k kihasználhatja az Azure AD **a nevében a** folyamatot, amely lehetővé teszi, hogy a webes API-k tooexchange hello számára egy másik access token toobe kimenő kérelmek szerepel egy bejövő jogkivonatot.  hello v2.0-végpontra a folyamat nevében ismertetett [részletesen Itt](active-directory-v2-protocols-oauth-on-behalf-of.md).

## <a name="mobile-and-native-apps"></a>Mobil- és natív alkalmazások
Eszköz telepített alkalmazások, többek között a mobil- és asztali alkalmazások, gyakran kell tooaccess háttér-szolgáltatásaihoz vagy webes API-k, amelyek adatokat tárolhatnak, és a feladatokat egy felhasználó nevében. Ezek az alkalmazások hello használatával adhat hozzá bejelentkezési és hitelesítési tooback-szolgáltatások [OAuth 2.0 hitelesítésikód-folyamata](active-directory-v2-protocols-oauth-code.md).

Ez a folyamat a hello app kap egy engedélyezési kód hello v2.0-végponttól hello felhasználó bejelentkezésekor. hello engedélyezési kód jelöli hello alkalmazás engedély toocall háttér-szolgáltatásaihoz hello van bejelentkezett felhasználó nevében. hello app továbbíthatja az OAuth 2.0 hozzáférési tokent, és egy frissítési jogkivonat hello háttérben hello engedélyezési kódot. hello alkalmazás használhat hello access token tooauthenticate tooWeb API-kat a HTTP-kérelmekre, és hello frissítési jogkivonat tooget új hozzáférési jogkivonatok használja, ha a régebbi hozzáférési jogkivonatok lejár.

![Natív alkalmazás hitelesítési folyamat](../../media/active-directory-v2-flows/convergence_scenarios_native.png)

## <a name="single-page-apps-javascript"></a>Egyoldalas alkalmazások (JavaScript)
Számos modern alkalmazások alkalmazás előtér írt elsősorban a JavaScript rendelkezik. Gyakran írás például AngularJS, az Ember.js vagy Durandal.js keretrendszer használatával. hello Azure AD v2.0-végponttól támogatja ezeket az alkalmazásokat hello segítségével [OAuth 2.0 típusú implicit engedélyezési folyamat](active-directory-v2-protocols-implicit.md).

Ez a folyamat a hello alkalmazás-jogkivonatokat kap közvetlenül az hello v2.0 engedélyezik a végponthoz, anélkül, hogy a kiszolgáló-kiszolgáló cseréjét. Hitelesítési logikát és a munkamenet vesz kezelése teljes mértékben hello JavaScript ügyfélprogramokban, de további lap átirányítások helyezi.

![Implicit hitelesítési folyamat](../../media/active-directory-v2-flows/convergence_scenarios_implicit.png)

toosee a folyamatot, próbálja ki valamelyik alkalmazás mintakódok hello a a [bevezetés](active-directory-appmodel-v2-overview.md#getting-started) szakasz.

## <a name="daemons-and-server-side-apps"></a>Démonok és kiszolgálóoldali alkalmazások
Az alkalmazások, amelyek hosszú futású folyamatokat, vagy egy felhasználói beavatkozás nélkül is szükség van egy tooaccess védett erőforrások, például webes API-k. Ezek az alkalmazások hitelesítheti és jogkivonatokhoz hello alkalmazás identitásával, nem pedig a felhasználó delegált identitása, az OAuth 2.0 hello ügyfél hitelesítő adatok folyamata.

Ez a folyamat a hello app kommunikál közvetlenül hello `/token` végpont tooobtain végpontok:

![Démon alkalmazás hitelesítési folyamat](../../media/active-directory-v2-flows/convergence_scenarios_daemon.png)

egy démon app toobuild dokumentációjában hello ügyfél hitelesítő adatait a a [bevezetés](active-directory-appmodel-v2-overview.md#getting-started) szakaszban, vagy adjon meg egy [.NET mintaalkalmazás](https://github.com/Azure-Samples/active-directory-dotnet-daemon-v2).
