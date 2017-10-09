---
title: "Az Azure Active Directory B2C: Tokenbeolvasás Android alkalmazással |} Microsoft Docs"
description: "Ez a cikk bemutatja, hogyan toocreate Android-alkalmazás, amely Azure Active Directory B2C toomanage felhasználói identitások AppAuth használ, valamint a felhasználók hitelesítésére."
services: active-directory-b2c
documentationcenter: android
author: parakhj
manager: krassk
editor: 
ms.assetid: d00947c3-dcaa-4cb3-8c2e-d94e0746d8b2
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: article
ms.date: 03/06/2017
ms.author: parakhj
ms.openlocfilehash: 0236398673115a34951f035cb1e73e89417abf86
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-b2c-sign-in-using-an-android-application"></a>Az Azure AD B2C: Bejelentkezés Android alkalmazás segítségével

hello Microsoft identitásplatformmal például OAuth2 és az OpenID Connect nyitott szabványok használja. Ez lehetővé teszi a fejlesztők tooleverage függvénytárat, a szolgáltatások toointegrate kívánnak. tooaid fejlesztők számára a más könyvtárak a platformot használnak, azt korábban írt néhány forgatókönyvek például a egy toodemonstrate hogyan tooconfigure 3. fél szalagtárak tooconnect toohello Microsoft identitásplatformmal. A legtöbb tárak, amelyek megvalósítják az [hello RFC6749 OAuth2 spec](https://tools.ietf.org/html/rfc6749) képes tooconnect toohello Microsoft Identity platform lesz.

> [!WARNING]
> A Microsoft nem biztosít azoknak a 3. fél szalagtárak, és nem végrehajtva a tárak áttekintése. Ez a minta, amely tesztelték AppAuth nevű alapszintű forgatókönyvekben hello Azure AD B2C-vel való kompatibilitás érdekében 3. fél szalagtárat használ. Problémákat és funkciókra vonatkozó kérések irányított toohello könyvtár nyílt forráskódú projekt kell lennie. Ellenőrizze a [Ez a cikk](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-libraries) további információt.  
>
>

Ha új tooOAuth2 vagy az OpenID Connect még a minta konfigurálási nem tehetik sok logika tooyou. Javasoljuk, hogy megnézi rövid [azt korábban itt dokumentált lehetőségektől hello protokoll – áttekintés](active-directory-b2c-reference-protocols.md).

## <a name="get-an-azure-ad-b2c-directory"></a>Az Azure AD B2C-címtár beszerzése

Az Azure AD B2C használatához létre kell hoznia egy címtárat vagy bérlőt. A címtárban tárolhatja az összes felhasználót, alkalmazást, csoportot és más elemeket. Ha még nem tette meg, [hozzon létre most egy B2C-címtárat](active-directory-b2c-get-started.md), mielőtt továbblépne.

## <a name="create-an-application"></a>Alkalmazás létrehozása

A következő lépésben toocreate egy alkalmazást a B2C-címtárban. Ez biztosítja, hogy kell-e az alkalmazással biztonságosan toocommunicate az Azure AD-információkat. Kövesse a mobilalkalmazások toocreate [ezeket az utasításokat](active-directory-b2c-app-registration.md). Ügyeljen arra, hogy:

* Tartalmaznak egy **Native Client** hello alkalmazásban.
* Másolás hello **Alkalmazásazonosító** , amely hozzárendelt tooyour alkalmazást. Ezt később lesz szüksége.
* Állítson be egy natív ügyfél **átirányítási URI-** (pl. com.onmicrosoft.fabrikamb2c.exampleapp://oauth/redirect). Később erre is szüksége lesz.

[!INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Házirendek létrehozása

Az Azure AD B2C-ben a felhasználói élményeket [szabályzatok](active-directory-b2c-reference-policies.md) határozzák meg. Az alkalmazás tartalmaz egy identitás-élmény: egy kombinált bejelentkezési és regisztrációs. Szüksége toocreate ezt a házirendet, lásd: a [házirendek áttekintésével foglalkozó cikkben](active-directory-b2c-reference-policies.md#create-a-sign-up-policy). Hello házirend létrehozásakor ügyeljen arra, hogy:

* Válassza ki a hello **megjelenített név** a szabályzatot az előfizetési attribútumaként.
* Válassza ki a hello **megjelenített név** és **Objektumazonosító** minden házirend alkalmazási jogcímet. Kiválaszthat egyéb jogcímeket is.
* Másolás hello **neve** egyes házirendek létrehozása után. Hello előtag nem rendelkezhet `b2c_1_`.  Hello házirendnév később lesz szüksége.

[!INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Miután létrehozta a házirendeket, készen áll a toobuild Ön az alkalmazást.

## <a name="download-hello-sample-code"></a>Hello mintakód letöltése

Egy Azure AD B2C AppAuth használó minta adtunk [a Githubon](https://github.com/Azure-Samples/active-directory-android-native-appauth-b2c). Töltse le a hello kódot, és futtassa. Akkor is gyorsan megismerkedhet a saját alkalmazás a saját Azure AD B2C konfigurációval hello hello utasításait követve [README.md](https://github.com/Azure-Samples/active-directory-android-native-appauth-b2c/blob/master/README.md).

hello minta olyan módosítás által biztosított hello minta [AppAuth](https://openid.github.io/AppAuth-Android/). Látogasson el a lap toolearn AppAuth és funkcióival kapcsolatos további.

## <a name="modifying-your-app-toouse-azure-ad-b2c-with-appauth"></a>Az alkalmazás toouse Azure AD B2C-vel rendelkező AppAuth módosítása

> [!NOTE]
> AppAuth támogatja az Android API 16 (Jellybean) vagy újabb verzió. Azt javasoljuk, API 23 vagy újabb verzió.
>

### <a name="configuration"></a>Konfiguráció

Az Azure AD B2C kommunikációs megadását hello derítenie URI vagy megadva hello engedélyezési végpont és a token-végpont URI-azonosítók konfigurálható. Mindkét esetben szüksége lesz a következő információ hello:

* Bérlő azonosítója (pl. contoso.onmicrosoft.com)
* Házirend neve (pl. B2C\_1\_SignUpIn)

Ha úgy dönt, tooautomatically hello engedélyezési és a token-végpont URI-azonosítók felderítése, a hello felderítési URI toofetch információk megadása szükséges. hello felderítési URI generálhatók hello bérlői lecserélésével\_azonosítója és hello házirend\_neve a következő URL-cím hello:

```java
String mDiscoveryURI = "https://login.microsoftonline.com/<Tenant_ID>/v2.0/.well-known/openid-configuration?p=<Policy_Name>";
```

Ezután hello engedélyezési és a token-végpont URI-k megszerzésére és AuthorizationServiceConfiguration objektumot létrehozni hello következő futtatásával:

```java
final Uri issuerUri = Uri.parse(mDiscoveryURI);
AuthorizationServiceConfiguration config;

AuthorizationServiceConfiguration.fetchFromIssuer(
    issuerUri,
    new RetrieveConfigurationCallback() {
      @Override public void onFetchConfigurationCompleted(
          @Nullable AuthorizationServiceConfiguration serviceConfiguration,
          @Nullable AuthorizationException ex) {
        if (ex != null) {
            Log.w(TAG, "Failed tooretrieve configuration for " + issuerUri, ex);
        } else {
            // service configuration retrieved, proceed tooauthorization...
        }
      }
  });
```

Felderítési tooobtain hello engedélyezési és a token-végpont URI-k helyett azt is megadhatja azokat explicit módon történő lecserélésével hello bérlői\_azonosítója és hello házirend\_neve hello URL-ben az alábbi:

```java
String mAuthEndpoint = "https://login.microsoftonline.com/<Tenant_ID>/oauth2/v2.0/authorize?p=<Policy_Name>";

String mTokenEndpoint = "https://login.microsoftonline.com/<Tenant_ID>/oauth2/v2.0/token?p=<Policy_Name>";
```

Futtassa a következő kód toocreate hello a AuthorizationServiceConfiguration objektum:

```java
AuthorizationServiceConfiguration config =
        new AuthorizationServiceConfiguration(name, mAuthEndpoint, mTokenEndpoint);

// perform hello auth request...
```

### <a name="authorizing"></a>Engedélyezése

Konfigurálja, vagy egy engedélyezési szolgáltatás konfigurációjának beolvasása után egy engedélyezési-kérelem lehet létrehozni. kérelem toocreate hello, szüksége lesz a következő információ hello:

* Ügyfél-azonosító (pl. 00000000-0000-0000-0000-000000000000)
* Átirányítási URI egy egyéni séma (pl. com.onmicrosoft.fabrikamb2c.exampleapp://oauthredirect)

Mindkét elemeket kell mentett Ha Ön volt [regisztrálja az alkalmazást](#create-an-application).

```java
AuthorizationRequest req = new AuthorizationRequest.Builder(
    config,
    clientId,
    ResponseTypeValues.CODE,
    redirectUri)
    .build();
```

Tekintse meg a toohello [AppAuth útmutató](https://openid.github.io/AppAuth-Android/) a hogyan toocomplete hello ki hello folyamat. Ha tooquickly kell egy működő alkalmazást az első lépései, tekintse meg [a minta](https://github.com/Azure-Samples/active-directory-android-native-appauth-b2c). Hello kövesse hello [README.md](https://github.com/Azure-Samples/active-directory-android-native-appauth-b2c/blob/master/README.md) tooenter a saját Azure AD B2C-konfiguráció.

Azt a rendszer mindig megnyitott toofeedback és javaslatok! Ha bármilyen nehézségbe ütközik az ebben a témakörben, vagy az ehhez a tartalomhoz javítására állnak, azt fogadjuk visszajelzéseit hello lap hello alján. A szolgáltatás kéréseket, vegye fel őket túl[UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c).

