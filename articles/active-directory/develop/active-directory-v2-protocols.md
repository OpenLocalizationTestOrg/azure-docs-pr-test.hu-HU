---
title: "az Azure AD v2.0 által támogatott hello engedélyezési protokollokról aaaLearn |} Microsoft Docs"
description: "Az útmutató tooprotocols, hello Azure AD v2.0-végponttól által támogatott."
services: active-directory
documentationcenter: 
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: 5fb4fa1b-8fc4-438e-b3b0-258d8c145f22
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/07/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: f90511b1880ff223f725c1f79df9f79bddccc7bf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# v2.0 protokoll - OAuth 2.0-s & OpenID Connect
hello v2.0-végponttól használhatja az Azure AD identity,--szolgáltatás az iparági szabványos protokollok, OpenID Connectet és az OAuth 2.0-s.  Szabványokkal kompatibilis hello szolgáltatás pedig finom eltérések vannak ezek a protokollok minden két közötti lehet.  hello információkat itt fog akkor hasznos, ha a választott toowrite a kódot közvetlenül elküldött & kezelési HTTP-kérelmekre, vagy használja a 3. fél nyissa meg a forrás könyvtár, nem pedig a nyílt forráskódú szalagtárak egyikével.
<!-- TODO: Need link toolibraries above -->

> [!NOTE]
> Nem minden Azure Active Directory forgatókönyvek és funkciók támogatják hello v2.0-végponttól.  toodetermine használatát hello v2.0-végpontra, olvassa el [v2.0 korlátozások](active-directory-v2-limitations.md).
>
>

## hello alapjai
Szinte minden OAuth & OpenID Connect adatfolyamok nincsenek négy felek hello exchange részt:

![OAuth 2.0 szerepkörök](../../media/active-directory-v2-flows/protocols_roles.png)

* Hello **engedélyezési Server** hello v2.0 végpontja.  Ez felelős hello felhasználói identitás biztosítása, megadása és hozzáférési tooresources visszavonásával és a jogkivonatok kiállítása.  Ez más néven hello identitásszolgáltató – biztonságosan kezelési semmit toodo hello felhasználói adatokat, a hozzáférést, és a hello bizalmi kapcsolatok egy folyamat felek között.
* Hello **erőforrás tulajdonosa** általában hello végfelhasználói van.  Hello fél hello adatok tulajdonosa, amely rendelkezik hello power tooallow harmadik felekkel tooaccess, adott adatok, vagy az erőforrás.
* Hello **OAuth ügyfél** az alkalmazás azonosítja az alkalmazás azonosítóját.  Akkor általában hello fél hello végfelhasználói kommunikál, és a jogkivonatok kér hello engedélyezési kiszolgáló.  hello ügyfél kell adható engedély tooaccess hello erőforrás hello erőforrás tulajdonosa.
* Hello **erőforrás-kiszolgáló** hello erőforrás vagy adatokat tartalmazó van.  Hello toosecurely helyszerepkörre, és hello OAuth ügyfél engedélyezési kiszolgáló megbízik, és használja a tulajdonosi access_tokens tooensure tooa erőforrás elérő engedélyezhetők.

## Alkalmazás regisztrálása
Minden alkalmazás által használt hello v2.0-végponttal kell regisztrálva van a következő toobe [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) előtt OAuth vagy az OpenID Connect használhatják.  hello az alkalmazásregisztrációs művelet során fog gyűjteni & néhány értékek tooyour alkalmazás hozzárendelése:

* Egy **alkalmazásazonosító** , amely egyedileg azonosítja az alkalmazást
* A **átirányítási URI-** vagy **csomagazonosítót** használt toodirect válaszok hátsó tooyour alkalmazás, amely lehet
* Néhány más forgatókönyvekre jellemző értékeket.

További részletekért megtudhatja, hogyan túl[alkalmazások regisztrálásának folyamatával](active-directory-v2-app-registration.md).

## Végpontok
Regisztrálás után hello app kommunikál az Azure AD küldött kérelmek toohello v2.0-végponttal:

```
https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize
https://login.microsoftonline.com/{tenant}/oauth2/v2.0/token
```

Ha hello `{tenant}` négy különböző értékek valamelyikét hajthatja végre:

| Érték | Leírás |
| --- | --- |
| `common` |Lehetővé teszi a felhasználók személyes Microsoft-fiókok és a munkahelyi vagy iskolai fiókok Azure Active Directory toosign hello alkalmazásba. |
| `organizations` |Lehetővé teszi, hogy csak az Azure Active Directory toosign munkahelyi vagy iskolai fiókkal rendelkező felhasználók hello alkalmazásba. |
| `consumers` |Lehetővé teszi, hogy csak a felhasználók a személyes Microsoft-fiókok (msa-t) toosign hello alkalmazásba. |
| `8eaef023-2b34-4da1-9baa-8bc8c9d6a490` vagy `contoso.onmicrosoft.com` |Lehetővé teszi, hogy csak egy adott Azure Active Directory-bérlő toosign a munkahelyi vagy iskolai fiókkal rendelkező felhasználók hello alkalmazásba.  Hello rövid tartományneve hello Azure AD-bérlő vagy hello bérlői guid-azonosító is használható. |

További információt az ezeket a végpontokat a toointeract válasszon az alábbi adott alkalmazás típust.

## Tokenek
OAuth 2.0 és az OpenID Connect hello v2.0 végrehajtása széles körű kihasználása tulajdonosi jogkivonatok, beleértve a tulajdonosi jogkivonatok JWTs-ként. Egy tulajdonosi jogkivonatot egy egyszerűsített biztonsági jogkivonatot, hogy biztosít hello "tulajdonos" hozzáférési tooa erőforrás védelemmel ellátva. Abban az értelemben a "tulajdonos" hello, akik hello token félre. Egy entitás először hitelesítenie kell magát az Azure AD tooreceive hello tulajdonosi jogkivonattal, ha hello szükséges lépések nem veszi toosecure hello lexikális elem szerepel az átvitel és a tárolási, bár hozzá, és egy nem kívánt félnek használják. Míg néhány biztonsági jogkivonatokat egy beépített mechanizmust meggátolja, hogy a nem hitelesített felek használja őket, a tulajdonosi jogkivonatok nem rendelkezik a mechanizmus, és kell szállítani, például a transport layer security (HTTPS) biztonságos csatorna. Egy tulajdonosi jogkivonatot egyértelmű hello küldött a rendszer, a man a hello középső támadás rosszindulatú fél tooacquire hello jogkivonat használatával, és használhassák az illetéktelen hozzáféréstől tooa védett erőforrás. hello ugyanazt biztonsági elveket alkalmazza, ha a tárolást, vagy a gyorsítótárazás tulajdonosi jogkivonatok későbbi használatra. Mindig győződjön meg arról, hogy az alkalmazás továbbítja, és biztonságos módon tárolja a tulajdonosi jogkivonatokhoz. További biztonsági szempontok a tulajdonosi jogkivonatok, lásd: [RFC 6750 szakasz 5](http://tools.ietf.org/html/rfc6750).

További részletek a jogkivonatok hello v2.0-végponttól használt különböző típusú érhető el [v2.0 jogkivonat végponthivatkozás hello](active-directory-v2-tokens.md).

## Protokollok
Ha most készen áll a toosee néhány példa kér, az alábbi oktatóanyagok hello egyik első lépései.  Mindegyik felel meg a tooa adott hitelesítési forgatókönyv.  Ha a megfelelő folyamat hello meg megállapításához segítségre van szüksége, tekintse meg [típusú alkalmazásokat hozhat létre a hello v2.0 hello](active-directory-v2-flows.md).

* [Mobileszköz- és natív alkalmazás OAuth 2.0-val hozhat létre.](active-directory-v2-protocols-oauth-code.md)
* [Build webes alkalmazások nyitott azonosítójú kapcsolódhatnak](active-directory-v2-protocols-oidc.md)
* [Egyetlen lap alkalmazások fejlesztheti az OAuth 2.0 Implicit Flow hello](active-directory-v2-protocols-implicit.md)
* [Démonok vagy a kiszolgáló oldalán folyamatok fejlesztheti az OAuth 2.0 ügyfél hitelesítő adatok áramlását hello](active-directory-v2-protocols-oauth-client-creds.md)
* [A Web API-t OAuth 2.0 más nevében Flow hello a jogkivonatok lekérésére](active-directory-v2-protocols-oauth-on-behalf-of.md)
