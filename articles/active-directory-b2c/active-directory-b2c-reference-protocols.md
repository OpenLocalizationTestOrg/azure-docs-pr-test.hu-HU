---
title: "Az Azure Active Directory B2C: Hitelesítési protokollok |} Microsoft Docs"
description: "Hogyan toobuild apps segítségével közvetlenül hello Azure Active Directory B2C által támogatott protokollok"
services: active-directory-b2c
documentationcenter: 
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: 5e407d0a-73a2-4d74-ac81-3aa6c31ddcee
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/07/2017
ms.author: dastrock
ms.openlocfilehash: 8fa4cbebe711841d410b3ae43b78f893c06d9b63
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# Az Azure AD B2C: Hitelesítési protokollok
Az Azure Active Directory B2C két iparági szabványos protokollok támogatása (az Azure AD B2C) biztosít az az alkalmazások szolgáltatásként identitás: OpenID Connectet és az OAuth 2.0-s. hello szolgáltatás szabványoknak megfelelő, de ezeket a protokollokat bármely két implementációja rendelkezhet finom eltérések vannak. 

hello információkat az útmutató akkor hasznos, ha közvetlenül küldésével és HTTP-kérelmek kezelése, nem pedig egy nyílt forráskódú könyvtár használatával írhatja a kódot. Azt javasoljuk, hogy olvassa el ezen a lapon az ahhoz, hogy mélyedjen el az adott protokollokat hello részleteit. De ha már ismeri az Azure AD B2C, elvégezheti a rögtön túl[protokoll hivatkozási útmutatók hello](#protocols).

<!-- TODO: Need link toolibraries above -->

## hello alapjai
Minden Azure AD B2C alkalmazó alkalmazásban kell regisztrálni a B2C-címtárban lévő hello toobe [Azure-portálon](https://portal.azure.com). hello az alkalmazásregisztrációs művelet során gyűjt, és hozzárendeli a néhány értékek tooyour alkalmazást:

* **Application ID** (Alkalmazásazonosító), amely egyedileg azonosítja az alkalmazást.
* A **átirányítási URI-** vagy **csomag azonosítója** használt toodirect válaszok hátsó tooyour alkalmazás, amely lehet.
* Néhány más forgatókönyvekre jellemző értékeket. További információt a további [hogyan tooregister az alkalmazás](active-directory-b2c-app-registration.md).

Az alkalmazás regisztrálása után azt kommunikál az Azure Active Directory (Azure AD) küldött kérelmek toohello végpont:

```
https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize
https://login.microsoftonline.com/{tenant}/oauth2/v2.0/token
```

Szinte minden OAuth és az OpenID Connect adatfolyamok a következők hello exchange részt négy felek:

![OAuth 2.0 szerepkörök](./media/active-directory-b2c-reference-protocols/protocols_roles.png)

* Hello **engedélyezési server** hello Azure AD-végpont. Semmi kapcsolódó toouser információkat és a hozzáférés biztonságosan kezeli. Kezeli a hello bizalmi kapcsolatok egy folyamat hello felek között is. Ez felelős hello felhasználói identitás ellenőrzése, megadása és hozzáférési tooresources visszavonásával és a jogkivonatok kiállítása. Akkor is hello identitásszolgáltató.

* Hello **erőforrás tulajdonosa** általában a végfelhasználó hello van. Hello fél hello adatok birtokló, és adott adatok vagy az erőforrás hello power tooallow harmadik felek tooaccess rendelkezik.

* Hello **OAuth ügyfél** az alkalmazás. Ez azonosítja az alkalmazás azonosítóját. Akkor általában hello entitás, amely a végfelhasználók interakciót. Azt is kér jogkivonatok hello engedélyezési kiszolgálótól. hello erőforrás tulajdonosa hello ügyfél engedélyt tooaccess hello erőforrás kell biztosítania.

* Hello **erőforrás-kiszolgáló** hello erőforrás vagy adatokat tartalmazó van. Hello engedélyezési megbízhatónak fogja tekinteni server toosecurely helyszerepkörre, és hello OAuth ügyfél. Tulajdonosi hozzáférési jogkivonatok tooensure tooa erőforrás elérő engedélyezhetők is használja.

## Házirendek
Késései az Azure AD B2C-házirendek olyan hello legfontosabb funkciókról hello szolgáltatást. Az Azure AD B2C házirendek bevezetésével kiterjeszti a hello szabványos OAuth 2.0-s és OpenID Connect protokollt használják. Ezek teszik lehetővé az Azure AD B2C tooperform sokkal több, mint az egyszerű hitelesítéshez és engedélyezéshez. 

Házirendek teljes leírása fogyasztói identitással kapcsolatos műveletet, beleértve a regisztráció, bejelentkezés, profil és szerkesztését. Házirendek meghatározása egy rendszergazda felhasználói felületén. A HTTP-hitelesítési kérelmek egy speciális lekérdezési paraméter segítségével végrehajthatók. 

Házirendek nincsenek szabványos OAuth 2.0 és az OpenID Connect, szolgáltatásait, hello idő toounderstand kell venni őket. További információkért lásd: hello [házirend referencia-útmutató az Azure AD B2C](active-directory-b2c-reference-policies.md).

## Tokenek
OAuth 2.0 és az OpenID Connect Azure AD B2C hello végrehajtásának lehetővé teszi a tulajdonosi jogkivonatok, beleértve a tulajdonosi jogkivonatok jelentésekként jelennek meg a JSON webes jogkivonatok (JWTs) használatának lehetőségét. Egy tulajdonosi jogkivonatot egy egyszerűsített biztonsági jogkivonatot, hogy biztosít hello "tulajdonos" hozzáférési tooa erőforrás védelemmel ellátva.

hello tulajdonosi, akik hello token félre. Az Azure AD először hitelesíteniük kell egy entitás előtt megkaphatja a tulajdonosi jogkivonattal. De ha hello szükséges lépések nem veszi toosecure hello lexikális elem szerepel az átvitel és a tárolási, azt is hozzá és egy nem kívánt entitás használja.

Néhány biztonsági jogkivonatokat rendelkezik beépített mechanizmust, amely megakadályozhatja, hogy a nem hitelesített felek a őket, de tulajdonosi jogkivonatok nem rendelkezik a mechanizmus. Azok a biztonságos csatornákat, például egy a transport layer security (HTTPS) kell szállítani. 

Egy tulajdonosi jogkivonatot kívül egy biztonságos csatornán kerül továbbításra, ha egy rosszindulatú entitás-átjárójának támadás tooacquire hello jogkivonatot használja, és toogain jogosulatlan hozzáférés tooa védett erőforrás használja. hello ugyanazt biztonsági elveket alkalmazza, ha a tulajdonosi jogkivonatok tárolt és gyorsítótárba helyezni későbbi használat. Mindig győződjön meg arról, hogy az alkalmazás továbbítja, és biztonságos módon tárolja a tulajdonosi jogkivonatokhoz.

További tulajdonosi jogkivonat biztonsági szempontjait, lásd: [RFC 6750 szakasz 5](http://tools.ietf.org/html/rfc6750).

További információ a különböző típusú hello jogkivonatokat, amelyek az Azure AD B2C találhatók [hello Azure AD-jogkivonatok referenciájából](active-directory-b2c-reference-tokens.md).

## Protokollok
Amikor készen áll tooreview néhány példa kér, megkezdheti a következő oktatóanyagok hello egyike. Tooa adott hitelesítési forgatókönyv mindegyike megfelel. Ha annak meghatározásakor, amelyben az Ön számára legmegfelelőbb segítségre van szüksége, tekintse meg [típusú alkalmazásokat hozhat létre az Azure AD B2C segítségével hello](active-directory-b2c-apps.md).

* [Mobil- és natív alkalmazások létrehozását OAuth 2.0 használatával](active-directory-b2c-reference-oauth-code.md)
* [Webalkalmazások OpenID Connect használatával összeállítása](active-directory-b2c-reference-oidc.md)
* [Egyoldalas alkalmazások hello OAuth 2.0 típusú implicit engedélyezési folyamat használata](active-directory-b2c-reference-spa.md)

