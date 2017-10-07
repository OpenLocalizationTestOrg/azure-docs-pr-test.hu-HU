---
title: "aaaChanges toohello az Azure AD v2.0-végponttól |} Microsoft Docs"
description: "Toohello app model v2.0 nyilvános előzetes protokollok végrehajtott módosítások leírását."
services: active-directory
documentationcenter: 
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: e6c5b891-0b5d-47dc-8184-5d0ef6a1a006
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/16/2016
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: d7b28a481e12d5dbbc4a10110193bdbd754f4929
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="important-updates-toohello-v20-authentication-protocols"></a>Fontos frissítések toohello v2.0 hitelesítési protokollok
Figyelmet fejlesztők! Hello keresztül következő két héten frissítjük néhány frissítések tooour v2.0 hitelesítési protokollok, amelyek a módosítások a próbaidőszak alatt írt alkalmazások számára megtörje jelentheti.  

## <a name="who-does-this-affect"></a>Aki befolyásolja ez?
Toouse hello v2.0 írt alkalmazások összevont hitelesítési végpontot

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize
```

További információ a v2.0-végponttól hello található [Itt](active-directory-appmodel-v2-overview.md).

Ha már létrehozta a buildet hello v2.0-végponttól használatával kódolás közvetlenül toohello v2.0 protokoll, az alkalmazások bármely az OpenID Connect vagy OAuth webes middlewares használatával, vagy más 3. fél szalagtárak tooperform hitelesítést használ, érdemes tootest előkészítve a projektek és később Ha a szükséges módosításokat.

## <a name="who-doesnt-this-affect"></a>Ki nem ez hatással van?
Hello éles Azure AD hitelesítési végpont, elleni írt alkalmazások

```
https://login.microsoftonline.com/common/oauth2/authorize
```

Ez a protokoll köve van beállítva, és fog nem léptek fel a módosításokat.

Továbbá ha az alkalmazás **csak** az ADAL-könyvtár tooperform hitelesítést használ, toochange semmit nem kell.  ADAL rendelkezik védett hello módosításokat az alkalmazását.  

## <a name="what-are-hello-changes"></a>Mik azok a hello módosításait?
### <a name="removing-hello-x5t-value-from-jwt-headers"></a>Hello x5t érték eltávolítása a JWT fejlécek
hello v2.0-végponttól használ JWT-jogkivonatokat széles körben, tartalmazó kapcsolódó metaadatokkal kapcsolatos hello token paraméterek fejlécszakasza.  Ha dekódolása hello fejléc az aktuális JWTs közül az egyik, találjuk hasonlót:

```
{ 
    "type": "JWT",
    "alg": "RS256",
    "x5t": "MnC_VZcATfM5pOYiJHMba9goEKY",
    "kid": "MnC_VZcATfM5pOYiJHMba9goEKY"
}
```

Ha mindkét hello "x5t" és "kid" Tulajdonságok hello nyilvános kulcsának azonosítása, amely kell használt toovalidate hello jogkivonat aláírása, lekért hello OpenID Connect metaadat-végpontjához.

Itt hajtunk hello módosítása tooremove hello "x5t" tulajdonság értéke.  Azonos mechanizmusok toovalidate jogkivonatokat, de a szoftverlicenceknek csak hello "kid" tulajdonság tooretrieve hello megfelelő nyilvános kulccsal, mint a megadott hello OpenID Connect protokoll toouse hello folytathatja. 

> [!IMPORTANT]
> **A feladat: Győződjön meg arról, hogy az alkalmazás nem függ a hello x5t érték hello megléte.**
> 
> 

### <a name="removing-profileinfo"></a>Profile_info eltávolítása
Korábban, hello v2.0-végponttal rendelkezik lett visszaadni a base64-kódolású JSON-objektum nevű token válaszok `profile_info`.  Amikor olyan hozzáférési jogkivonatot kérnek hello v2.0-végpontra vonatkozó kérelmet küld:

```
https://login.microsoftonline.com/common/oauth2/v2.0/token
```

hello válasz JSON-objektum a következő hello jelenne meg:

```
{ 
    "token_type": "Bearer",
    "expires_in": 3599,
    "scope": "https://outlook.office.com/mail.read",
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
    "refresh_token": "OAAABAAAAiL9Kn2Z27UubvWFPbm0gL...",
    "profile_info": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
}
```

Hello `profile_info` hello hello alkalmazás – a megjelenített név, Utónév, Vezetéknév, e-mail cím, azonosítója, és így tovább a bejelentkezett felhasználó értéket tartalmaz információt.  Elsősorban, hello `profile_info` token-gyorsítótárazási volt használva, és megjeleníti a célra.

Most megszüntetjük hello `profile_info` érték –, de ne aggódjon, Microsoft továbbra is ad az információk toodevelopers némileg eltérő helyen.  Ahelyett, hogy `profile_info`, hello v2.0-végponttól most visszaállítja egy `id_token` minden token válaszként:

```
{ 
    "token_type": "Bearer",
    "expires_in": 3599,
    "scope": "https://outlook.office.com/mail.read",
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
    "refresh_token": "OAAABAAAAiL9Kn2Z27UubvWFPbm0gL...",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
}
```

Előfordulhat, hogy dekódolása és elemezni a hello id_token tooretrieve hello profile_info kapott adatokat.  hello id_token egy JSON webes jogkivonat (JWT), az OpenID Connect által megadott tartalom.  hello kódot úgy legyen nagyon hasonló – tooextract hello közel egyszerűen szegmens (hello törzs) hello id_token és base64 dekódolni a tooaccess hello JSON-objektum belül.

Hello keresztül következő két héten kell kódaláírással az alkalmazás tooretrieve hello felhasználói adatok vagy hello `id_token` vagy `profile_info`; attól jelen.  Ily módon módosításkor hello az alkalmazás zökkenőmentesen kezelni tud a hello áttűnés `profile_info` túl`id_token` megszakítás nélkül.

> [!IMPORTANT]
> **A feladat: Győződjön meg arról, hogy az alkalmazás nem függ a hello hello megléte `profile_info` érték.**
> 
> 

### <a name="removing-idtokenexpiresin"></a>Id_token_expires_in eltávolítása
Hasonló túl`profile_info`, is megszüntetjük hello `id_token_expires_in` válaszok paraméter.  Korábban, a hello v2.0-végpontra vonatkozó értéket visszaadnia `id_token_expires_in` minden egyes id_token válasz, például egy engedélyezés válaszul együtt:

```
https://myapp.com?id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...&id_token_expires_in=3599...
```

Vagy a token válasz:

```
{ 
    "token_type": "Bearer",
    "id_token_expires_in": 3599,
    "scope": "openid",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
    "refresh_token": "OAAABAAAAiL9Kn2Z27UubvWFPbm0gL...",
    "profile_info": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
}
```

Hello `id_token_expires_in` érték azt jelentené hello hány másodpercig hello id_token érvényes marad.  Most, megszüntetjük hello `id_token_expires_in` teljesen érték.  Ehelyett használhat hello OpenID Connect standard `nbf` és `exp` jogcímek egy id_token tooexamine hello érvényességét.  Lásd: hello [v2.0 jogkivonat referenciái](active-directory-v2-tokens.md) jogcímek olvashat.

> [!IMPORTANT]
> **A feladat: Győződjön meg arról, hogy az alkalmazás nem függ a hello hello megléte `id_token_expires_in` érték.**
> 
> 

### <a name="changing-hello-claims-returned-by-scopeopenid"></a>Hatókör által visszaadott hello jogcímek módosítása = openid
Ez a változás legjelentősebb – hello ténylegesen, az hatással szinte minden hello v2.0-végponttól alkalmazó alkalmazásban.  Sok alkalmazások küldési kérelmek toohello v2.0-végponttól hello segítségével `openid` hatókörét, például:

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=...
&redirect_uri=...
&response_mode=form_post
&response_type=id_token
&scope=openid offline_access https://outlook.office.com/mail.read
```

Ma, ha engedélyezi a hello felhasználói a hello kapcsolatos jóváhagyásról `openid` hatókör, az alkalmazás fogadása számos olyan információt hello felhasználóról id_token eredő hello.  Ezek a jogcímek tartalmazhatják a neve, elsődleges felhasználónév, e-mail címét, objektumazonosító: és több.

Ez a frissítés, azt módosítani hello információt, hogy hello `openid` hatókör számára biztosítja a alkalmazást a hozzáférést, az OpenID Connect specification hello toobetter comform.  Hello `openid` hatókör fog csak felhasználónak lehetővé teszi az alkalmazás toosign hello és hello felhasználó alkalmazásspecifikus azonosítót kap, hello `sub` hello id_token minősítése.  jogcím szerepel egy id_token hello rendelkező csak hello `openid` nyújtott lesz nem tartalmaz személyazonosításra alkalmas adatok.  Példa id_token jogcímeket a következők:

```
{ 
    "aud": "580e250c-8f26-49d0-bee8-1c078add1609",
    "iss": "https://login.microsoftonline.com/b9410318-09af-49c2-b0c3-653adc1f376e/v2.0 ",
    "iat": 1449520283,
    "nbf": 1449520283,
    "exp": 1449524183,
    "nonce": "12345",
    "sub": "MF4f-ggWMEji12KynJUNQZphaUTvLcQug5jdF2nl01Q",
    "tid": "b9410318-09af-49c2-b0c3-653adc1f376e",
    "ver": "2.0",
}
```

Tooobtain személyes azonosításra alkalmas adatokat (PII) hello felhasználóról az alkalmazásban, az alkalmazás fogja kell hello felhasználói toorequest további engedélyek.  Vezetjük be két új hatókört támogatása a hello OpenID Connect spec – hello `email` és `profile` hatókörök – amelyek toodo így.

* Hello `email` hatóköre nagyon egyszerű – lehetővé teszi az alkalmazás hozzáférési toohello felhasználó elsődleges e-mail címét keresztül hello `email` hello id_token a jogcímek.  Vegye figyelembe, hogy hello `email` jogcím nem mindig kerül be a id_tokens – csak akkor lesz része Ha hello profil érhető el.
* Hello `profile` objektumazonosító hatókör számára biztosítja az alkalmazás hozzáférési tooall más hello felhasználó – a neve, az elsődleges felhasználónév alapvető információkat, és így tovább.

Ez lehetővé teszi, hogy toocode minimális nyilvánosságra módon – az alkalmazás megkérheti hello felhasználói adatokat, hogy az alkalmazás futtatásához szükséges toodo feladata elvégzéséhez egyszerűen hello készlete esetében.  Ha azt szeretné, hogy az alkalmazás jelenleg fogadó felhasználói adatok teljes készletét hello első toocontinue, a hitelesítési kérések bele kell foglalni a három hatóköröket:

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=...
&redirect_uri=...
&response_mode=form_post
&response_type=id_token
&scope=openid profile email offline_access https://outlook.office.com/mail.read
```

Az alkalmazás megkezdődhet az hello küldése `email` és `profile` azonnal hatókörök és hello v2.0-végponttól elfogadja a két hatóköröket és megkezdéséhez engedélyek kér a felhasználóktól, szükség esetén.  Azonban a hello hello hello értelmezésének változása `openid` hatókör nem lépnek érvénybe néhány hétig.

> [!IMPORTANT]
> **A feladat: hello hozzáadása `profile` és `email` hatóköröket, ha az alkalmazás használatához hello felhasználó adatait.**  Vegye figyelembe, hogy adal-t fog tartalmazni mindkét engedéllyel a kérelmek alapértelmezés szerint. 
> 
> 

### <a name="removing-hello-issuer-trailing-slash"></a>Hello kibocsátó záró perjelet eltávolítása.
Korábban a v2.0-végponttól hello jogkivonatok megjelenő hello kibocsátó érték tartott hello képernyő

```
https://login.microsoftonline.com/{some-guid}/v2.0/
```

Ha hello guid lett hello tenantId hello jogkivonatot kibocsátó hello Azure AD-bérlő.  A módosítások hello kibocsátó érték válik.

```
https://login.microsoftonline.com/{some-guid}/v2.0 
```

mindkét jogkivonatokat, hello OpenID Connect felderítési dokumentumban.

> [!IMPORTANT]
> **A feladat: Győződjön meg arról, hogy az alkalmazás fogad hello kibocsátó érték vagy záró perjelet anélkül kibocsátó érvényesítése során.**
> 
> 

## <a name="why-change"></a>Miért is megváltozik?
hello elsődleges kifejlesztésének vezet be, ezeket a módosításokat az OpenID Connect szabvány hello megfelelő toobe.  Mivel OpenID Connect megfelelő, a Microsoft identity szolgáltatásokkal és az egyéb identitás-szolgáltatások hello iparági integrálásával toominimize különbségei Reméljük.  Szeretnénk tooenable fejlesztők toouse a kedvenc nyílt forráskódú hitelesítési tárakat tooalter hello szalagtárak tooaccommodate Microsoft különbségek nélkül.

## <a name="what-can-you-do"></a>Mire szolgál?
Mai megkezdheti a fent leírt hello végrehajtott módosítások elvégzése.  Azonnal kell:

1. **Távolítsa el az összes függőségét hello `x5t` fejléc paraméter.**
2. **A hello áttűnés kezelésére `profile_info` túl`id_token` token válaszokban.**
3. **Távolítsa el az összes függőségét hello `id_token_expires_in` válasz paraméter.**
4. **Adja hozzá a hello `profile` és `email` hatókörök tooyour alkalmazást, ha az alkalmazás alapszintű felhasználói információra van szüksége.**
5. **Fogadja el vagy záró perjelet anélkül jogkivonatokba kibocsátó értékeket.**

A [v2.0 protokoll dokumentációját](active-directory-v2-protocols.md) már be van frissített tooreflect ezeket a változásokat, így használhatja referenciaként létrehozásában, frissítse a kódot.

Ha hello hatókörön hello változások további kérdése van, adjon érzi, hogy szabad tooreach toous a Twitteren, out @AzureAD.

## <a name="how-often-will-protocol-changes-occur"></a>Milyen gyakran történik a protokoll módosításait?
Bármely további megtörje változik toohello hitelesítési protokollok nem várható.  Ezeket a módosításokat, azokat egy kiadási szándékosan azt vannak kötegelése, így nem kell a frissítési folyamat az ilyen típusú toogo újra bármikor elérhető.  Természetesen folytatjuk tooadd szolgáltatások toohello v2.0 hitelesítési szolgáltatás, amely kihasználhatja az összevont van, de ezeket a módosításokat adalékanyag és nem a meglévő kódot sortörés.

Végül tapasztalatairól toosay Köszönjük, hogy dolgot kipróbálásánál hello próbaidőszak alatt.  hello elemzések és a korai támogatók véleményeket hasznos információt eddigi, és Reméljük, hogy a vélemények és ötleteket tooshare fogja folytatni.

Örömmel kódolási!

a Microsoft Identity osztás hello

