---
title: "Hitelesítésikód-folyamata – az Azure AD B2C |} Microsoft Docs"
description: "Ismerje meg, hogyan toobuild webalkalmazások Azure AD B2C és az OpenID Connect hitelesítési protokoll használatával."
services: active-directory-b2c
documentationcenter: 
author: saeedakhter-msft
manager: krassk
editor: parakhj
ms.assetid: c371aaab-813a-4317-97df-b62e2f53d865
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/16/2017
ms.author: saeedakhter-msft
ms.openlocfilehash: 6bf9d37310bd45b39bda346441413556f9fd4fdc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-oauth-20-authorization-code-flow"></a>Az Azure Active Directory B2C: OAuth 2.0 hitelesítésikód-folyamata
Egy eszköz toogain hozzáférés tooprotected-erőforrások, például webes API-k a telepített alkalmazások hello OAuth 2.0 hitelesítésengedélyezési kód használható. OAuth 2.0 (az Azure AD B2C) végrehajtásának hello Azure Active Directory B2C segítségével is hozzáadhat a regisztráció, bejelentkezés és egyéb identitás-kezelési feladatainak tooyour hordozható és asztali alkalmazások. Ez a cikk nyelvfüggetlen. Hello cikk azt ismerteti hogyan toosend és HTTP-üzenetek fogadása bármely nyílt forráskódú kódtárai használata nélkül.

<!-- TODO: Need link toolibraries -->

OAuth 2.0 hitelesítésikód-folyamata hello ismertetett [hello OAuth 2.0-s szabvány 4.1 szakasz](http://tools.ietf.org/html/rfc6749). Használhatja a hitelesítéshez és engedélyezéshez a legtöbb alkalmazás típusok, beleértve a [webalkalmazások](active-directory-b2c-apps.md#web-apps) és [natívan telepített alkalmazásokat](active-directory-b2c-apps.md#mobile-and-native-apps). Használhatja az OAuth 2.0 hitelesítési kód folyamat toosecurely szerezni hello *hozzáférési jogkivonatok* az alkalmazásokat, amelyek lehet által védett erőforrások használt tooaccess egy [engedélyezési server](active-directory-b2c-reference-protocols.md#the-basics).

Ez a cikk foglalkozik a hello **nyilvános ügyfelek** OAuth 2.0 hitelesítésikód-folyamata. Nyilvános ügyfél, minden ügyfél-alkalmazás, amely nem lehet a megbízható toosecurely titkos jelszó hello integritásának fenntartása. Ez magában foglalja a mobilalkalmazások, az asztali alkalmazások és a tulajdonképpen bármely olyan alkalmazás fut egy eszközön, és tooget hozzáférési jogkivonatok kell. 

> [!NOTE]
> tooadd identity management tooa webalkalmazást az Azure AD B2C, használja a [OpenID Connect](active-directory-b2c-reference-oidc.md) OAuth 2.0 helyett.

Az Azure AD B2C hello szabványos OAuth 2.0 forgalomáramlás több, mint az egyszerű hitelesítés és engedélyezés toodo terjeszti ki. Hello okozna [házirend paraméter](active-directory-b2c-reference-policies.md). A beépített házirendek, használhatja az OAuth 2.0-s tooadd felhasználói tooyour alkalmazást észlel, mint regisztrációs, bejelentkezési és profilkezelést. Ez a cikk azt mutatja be ezen észlel, a natív alkalmazások toouse OAuth 2.0-s és a házirendek tooimplement. Azt is bemutatja, hogyan tooget hozzáférési jogkivonatok eléréséhez webes API-khoz.

A minta Azure AD B2C-címtár használjuk hello példa HTTP-kérelmek ebben a cikkben, **fabrikamb2c.onmicrosoft.com**. Is használhatja a mintaalkalmazás és a házirendeket. Megpróbálhatja hello kérelmek saját kezűleg ezen értékek használatával, vagy azokat lecserélheti a saját értékeit.
Ismerje meg, hogyan túl[beolvasása a saját Azure AD B2C directory, az alkalmazás és a házirendek](#use-your-own-azure-ad-b2c-directory).

## <a name="1-get-an-authorization-code"></a>1. Az engedélyezési kód beolvasása
hello hitelesítésikód-folyamata kezdődik arra utasíthatja a hello felhasználói toohello hello ügyfél `/authorize` végpont. Ez része hello interaktív hello folyamata, ahol a hello felhasználó végrehajtja a műveletet. Ebben a kérelemben hello ügyfél jelzi a hello `scope` paraméter hello engedélyeit, hogy kell-e a hello felhasználó tooacquire. A hello `p` paraméter azt jelzi, hogy hello házirend tooexecute. hello következő három példák (olvashatóság érdekében sortöréssel) minden használja egy másik házirendet.

### <a name="use-a-sign-in-policy"></a>A bejelentkezési házirend
```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code
&redirect_uri=urn%3Aietf%3Awg%3Aoauth%3A2.0%3Aoob
&response_mode=query
&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&p=b2c_1_sign_in
```

### <a name="use-a-sign-up-policy"></a>A regisztrációs szabályzatban használata
```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code
&redirect_uri=urn%3Aietf%3Awg%3Aoauth%3A2.0%3Aoob
&response_mode=query
&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&p=b2c_1_sign_up
```

### <a name="use-an-edit-profile-policy"></a>Egy házirend-profil szerkesztése
```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code
&redirect_uri=urn%3Aietf%3Awg%3Aoauth%3A2.0%3Aoob
&response_mode=query
&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&p=b2c_1_edit_profile
```

| Paraméter | Kötelező? | Leírás |
| --- | --- | --- |
| client_id |Szükséges |hello Alkalmazásazonosító hozzárendelt hello tooyour alkalmazás [Azure-portálon](https://portal.azure.com). |
| response_type |Szükséges |hello válaszának típusa, amely tartalmazza `code` a hello hitelesítésikód-folyamata. |
| redirect_uri |Szükséges |hello átirányítási URI-címe, az alkalmazás, ahol hitelesítési válaszokat küldött és az alkalmazás által fogadott. Az pontosan egyeznie kell hello átirányítási URI-azonosítók regisztrált hello portálon, azzal a különbséggel, hogy az URL-kódolású kell lennie. |
| Hatókör |Szükséges |Hatókörök szóközökkel elválasztott listája. Egy hatókör érték tooAzure Active Directory (Azure AD) mindkét hello engedélyeket, hogy kérnek. Hello hatókör azt jelzi, hogy az alkalmazás egy hozzáférési jogkivonatot, amely használható a saját szolgáltatás vagy webes API-t, mert által képviselt azonosító hello ügyfélprogrammal hello ugyanazon ügyfél-azonosítót.  Hello `offline_access` hatókör azt jelzi, hogy kell-e az alkalmazást egy frissítési jogkivonat a hosszú élettartamú hozzáférés tooresources. Is használhatja hello `openid` hatókör toorequest egy Azure AD B2C azonosító jogkivonatot. |
| response_mode |Ajánlott |hello metódus toosend hello eredményül kapott engedélyezési kód hátsó tooyour alkalmazás használatát. Ez lehet a `query`, `form_post`, vagy `fragment`. |
| state |Ajánlott |Hello token válaszként visszaadott hello kérelemben szereplő érték. Bármilyen tartalmat, hogy toouse karakterlánc lehet. Általában egy véletlenszerűen generált egyedi érték használata esetén tooprevent webhelyközi kérések hamisításának megakadályozása támadásokat. hello is állapota hello felhasználói állapot hello alkalmazásban használt tooencode információ előtt hello hitelesítési kérelmet. Például a hello lap hello felhasználói volt, vagy hello végrehajtott házirend. |
| P |Szükséges |hello házirendet, amely végrehajtja a rendszer. Az hello a házirend nevét, amely a Azure AD B2C-címtárban jön létre. hello házirend név-érték előtaggal kell kezdődnie **b2c\_1\_**. További információ a házirendek, toolearn lásd [beépített házirendek az Azure AD B2C](active-directory-b2c-reference-policies.md). |
| parancssor |Optional |hello típusa a felhasználói beavatkozás szükséges. Hello egyetlen érvényes érték jelenleg `login`, amely kényszeríti hello felhasználói tooenter adott kérelem hitelesítő adataikat. Egyszeri bejelentkezés nem lépnek érvénybe. |

Ezen a ponton hello felhasználónak kapcsolatba toocomplete hello házirend munkafolyamat. Ez lehet, hogy magában hello felhasználói írja be a felhasználónevét és jelszavát, bejelentkezés egy közösségi identitás regisztrál a hello könyvtár, vagy több lépésből áll. Felhasználói műveletek attól függ, hogyan a hello házirend lett meghatározva.

Hello felhasználói hello házirend befejezése után az Azure AD adja vissza egy válasz tooyour alkalmazást használt hello érték `redirect_uri`. Hello megadott hello módszer használ `response_mode` paraméter. van, hello válasz pontosan azonos hello az egyes hello felhasználói művelet esetek, független hello házirend, hogy végre lett hajtva.

A sikeres válasz használó `response_mode=query` dolgozunk:

```
GET urn:ietf:wg:oauth:2.0:oob?
code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...        // hello authorization_code, truncated
&state=arbitrary_data_you_can_receive_in_the_response                // hello value provided in hello request
```

| Paraméter | Leírás |
| --- | --- |
| Kód |a kért alkalmazás hello engedélyezési kód hello. hello app hello engedélyezési kód toorequest olyan hozzáférési jogkivonatot használja a cél erőforráson. Hitelesítési kódok nagyon rövid élettartamú. Ezek általában körülbelül 10 perc múlva lejár. |
| state |Lásd az előző szakaszban hello hello tábla hello teljes leírását. Ha egy `state` paraméter szerepel hello ugyanazt az értéket meg kell jelennie a hello válasz hello kérelem. hello app győződjön meg arról, hogy hello `state` hello kérelem-válasz szereplő értékek azonosak. |

Hibaválaszok is küldhetők toohello átirányítási URI-t, hogy hello alkalmazást is kezeli azokat megfelelően:

```
GET urn:ietf:wg:oauth:2.0:oob?
error=access_denied
&error_description=The+user+has+cancelled+entering+self-asserted+information
&state=arbitrary_data_you_can_receive_in_the_response
```

| Paraméter | Leírás |
| --- | --- |
| error |Egy hiba kód karakterlánc, amely a jelentkező hibákról tooclassify hello típusok használhatók. Hello karakterlánc tooreact tooerrors is használhatja. |
| error_description |Egy adott hibaüzenet, melyek segíthetnek hello hitelesítési hiba okának azonosításához. |
| state |Lásd a táblázat megelőző hello hello teljes leírását. Ha egy `state` paraméter szerepel hello ugyanazt az értéket meg kell jelennie a hello válasz hello kérelem. hello app győződjön meg arról, hogy hello `state` hello kérelem-válasz szereplő értékek azonosak. |

## <a name="2-get-a-token"></a>2. A jogkivonat beolvasása
Most, hogy az engedélyezési kód jut hozzá, akkor is beválthatja hello `code` a token toohello szánt erőforrás küldött POST-kérelmet toohello `/token` végpont. Az Azure AD B2C-ben hello csak a kérhet egy token erőforrás az alkalmazás saját webes API-t. hello egyezmény használt token tooyourself kér az alkalmazás ügyfél-azonosítója hello hatóköreként toouse:

```
POST fabrikamb2c.onmicrosoft.com/oauth2/v2.0/token?p=b2c_1_sign_in HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=authorization_code&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob

```

| Paraméter | Kötelező? | Leírás |
| --- | --- | --- |
| P |Szükséges |hello házirend, de a használt tooacquire hello engedélyezési kódot. A kérelem nem használhatja másik szabályt. Vegye figyelembe, hogy hozzáadta a paraméter toohello *lekérdezési karakterlánc*, és nem hello POST törzsében. |
| client_id |Szükséges |hello Alkalmazásazonosító hozzárendelt hello tooyour alkalmazás [Azure-portálon](https://portal.azure.com). |
| grant_type |Szükséges |a támogatás hello típusát. Hello hitelesítésikód-folyamata, hello engedélytípus kell `authorization_code`. |
| Hatókör |Ajánlott |Hatókörök szóközökkel elválasztott listája. Egy hatókör érték azt jelzi, hogy mindkét AD tooAzure hello engedélyeket, hogy kérnek. Hello hatókör azt jelzi, hogy az alkalmazás egy hozzáférési jogkivonatot, amely használható a saját szolgáltatás vagy webes API-t, mert által képviselt azonosító hello ügyfélprogrammal hello ugyanazon ügyfél-azonosítót.  Hello `offline_access` hatókör azt jelzi, hogy kell-e az alkalmazást egy frissítési jogkivonat a hosszú élettartamú hozzáférés tooresources.  Is használhatja hello `openid` hatókör toorequest egy Azure AD B2C azonosító jogkivonatot. |
| Kód |Szükséges |hello engedélyezési kód hello folyamat első szakasza hello beszerzett. |
| redirect_uri |Szükséges |hello átirányítási URI-címe, amelyre kapta hello engedélyezési kód hello alkalmazás. |

A sikeres token válasz így néz ki:

```
{
    "not_before": "1442340812",
    "token_type": "Bearer",
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "scope": "90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access",
    "expires_in": "3600",
    "refresh_token": "AAQfQmvuDy8WtUv-sd0TBwWVQs1rC-Lfxa_NDkLqpg50Cxp5Dxj0VPF1mx2Z...",
}
```
| Paraméter | Leírás |
| --- | --- |
| not_before |hello idő, mely hello jogkivonat érvényes, időbeli kortartományon. |
| token_type |hello token objektumtípus-érték. hello csak adja meg, hogy az Azure AD által támogatott tulajdonosi-e. |
| access_token |hello JSON webes jogkivonat (JWT) az Ön által kért aláírva. |
| Hatókör |hello token hello hatókörök esetén érvényes. Is használhatja hatókörök toocache jogkivonatok későbbi használatra. |
| expires_in |Hello, hogy mennyi ideig hello jogkivonat érvénytelen (másodpercben). |
| refresh_token |Az OAuth 2.0-s frissítési jogkivonat. hello app használhatja a token tooacquire további jogkivonatok hello aktuális jogkivonat lejárata után is. Frissítési jogkivonatok hosszú élettartamú. Használhatja őket tooretain hozzáférés tooresources huzamosabb ideig. További információkért lásd: hello [Azure AD B2C-jogkivonatok referenciájából](active-directory-b2c-reference-tokens.md). |

Hibaválaszok néznek ki:

```
{
    "error": "access_denied",
    "error_description": "hello user revoked access toohello app.",
}
```

| Paraméter | Leírás |
| --- | --- |
| error |Egy hiba kód karakterlánc, amely a jelentkező hibákról tooclassify hello típusok használhatók. Hello karakterlánc tooreact tooerrors is használhatja. |
| error_description |Egy adott hibaüzenet, melyek segíthetnek hello hitelesítési hiba okának azonosításához. |

## <a name="3-use-hello-token"></a>3. Hello jogkivonat használata
Most, hogy olyan hozzáférési jogkivonatot már sikeresen szerezte be, használhatja hello token kérelmek tooyour webes API-k belefoglalja azt hello `Authorization` fejléc:

```
GET /tasks
Host: https://mytaskwebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

## <a name="4-refresh-hello-token"></a>4. Hello token frissítése
Hozzáférési jogkivonatok és azonosító-jogkivonatokat, amelyek rövid élettartamú. Után, frissítenie kell őket toocontinue tooaccess erőforrásokat. toodo, küldje el a POST-kérelmet egy másik toohello `/token` végpont. Most, adja meg a hello `refresh_token` helyett hello `code`:

```
POST fabrikamb2c.onmicrosoft.com/oauth2/v2.0/token?p=b2c_1_sign_in HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=refresh_token&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access&refresh_token=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob
```

| Paraméter | Kötelező? | Leírás |
| --- | --- | --- |
| P |Szükséges |hello házirend, de a használt tooacquire hello eredeti frissítési jogkivonat. A kérelem nem használhatja másik szabályt. Vegye figyelembe, hogy hozzáadta a paraméter toohello *lekérdezési karakterlánc*, és nem hello POST törzsében. |
| client_id |Ajánlott |hello Alkalmazásazonosító hozzárendelt hello tooyour alkalmazás [Azure-portálon](https://portal.azure.com). |
| grant_type |Szükséges |a támogatás hello típusát. Hello hitelesítésikód-folyamata ezen szakasza hello engedélytípus kell `refresh_token`. |
| Hatókör |Ajánlott |Hatókörök szóközökkel elválasztott listája. Egy hatókör érték azt jelzi, hogy mindkét AD tooAzure hello engedélyeket, hogy kérnek. Hello hatókör azt jelzi, hogy az alkalmazás egy hozzáférési jogkivonatot, amely használható a saját szolgáltatás vagy webes API-t, mert által képviselt azonosító hello ügyfélprogrammal hello ugyanazon ügyfél-azonosítót.  Hello `offline_access` hatókör azt jelzi, hogy az alkalmazás a hosszú élettartamú hozzáférés tooresources kell egy frissítési jogkivonat.  Is használhatja hello `openid` hatókör toorequest egy Azure AD B2C azonosító jogkivonatot. |
| redirect_uri |Optional |hello átirányítási URI-címe, amelyre kapta hello engedélyezési kód hello alkalmazás. |
| refresh_token |Szükséges |hello eredeti frissítési jogkivonat hello folyamat második szakasza hello beszerzett. |

A sikeres token válasz így néz ki:

```
{
    "not_before": "1442340812",
    "token_type": "Bearer",
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "scope": "90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access",
    "expires_in": "3600",
    "refresh_token": "AAQfQmvuDy8WtUv-sd0TBwWVQs1rC-Lfxa_NDkLqpg50Cxp5Dxj0VPF1mx2Z...",
}
```
| Paraméter | Leírás |
| --- | --- |
| not_before |hello idő, mely hello jogkivonat érvényes, időbeli kortartományon. |
| token_type |hello token objektumtípus-érték. hello csak adja meg, hogy az Azure AD által támogatott tulajdonosi-e. |
| access_token |hello jwt-t az Ön által kért aláírva. |
| Hatókör |hello token hello hatókörök esetén érvényes. Is használhatja hello hatókörök toocache jogkivonatok későbbi használatra. |
| expires_in |Hello, hogy mennyi ideig hello jogkivonat érvénytelen (másodpercben). |
| refresh_token |Az OAuth 2.0-s frissítési jogkivonat. hello app használhatja a token tooacquire további jogkivonatok hello aktuális jogkivonat lejárata után is. Frissítési jogkivonatok hosszú élettartamú, és hosszabb ideig használt tooretain hozzáférés tooresources lehet. További információkért lásd: hello [Azure AD B2C-jogkivonatok referenciájából](active-directory-b2c-reference-tokens.md). |

Hibaválaszok néznek ki:

```
{
    "error": "access_denied",
    "error_description": "hello user revoked access toohello app.",
}
```

| Paraméter | Leírás |
| --- | --- |
| error |Egy hiba kód karakterlánc, amely a jelentkező hibákról tooclassify típusok használhatók. Hello karakterlánc tooreact tooerrors is használhatja. |
| error_description |Egy adott hibaüzenet, melyek segíthetnek hello hitelesítési hiba okának azonosításához. |

## <a name="use-your-own-azure-ad-b2c-directory"></a>Használja a saját Azure AD B2C-címtár
Ezek a kérelmek magát, a következő teljes hello tootry lépéseket. Cserélje le a hello példaértékeket a saját értékekkel cikkben használtuk.

1. [Az Azure AD B2C címtár létrehozása](active-directory-b2c-get-started.md). A könyvtár hello nevét használja hello kérésekben.
2. [Hozzon létre egy alkalmazást](active-directory-b2c-app-registration.md) tooobtain Azonosítóját és egy átirányítási URI-t. Egy natív ügyfél tartalmazza az alkalmazásban.
3. [A házirendek létrehozása](active-directory-b2c-reference-policies.md) tooobtain a házirend nevét.

