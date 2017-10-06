---
title: "IOS alkalmazás - Azure AD B2C segítségével tokenbeolvasás |} Microsoft Docs"
description: "Ez a cikk bemutatja, hogyan toocreate egy iOS-alkalmazást, amely Azure Active Directory B2C toomanage felhasználói identitások AppAuth használ, valamint a felhasználók hitelesítésére."
services: active-directory-b2c
documentationcenter: ios
author: saeedakhter-msft
manager: krassk
editor: parakhj
ms.assetid: d818a634-42c2-4cbd-bf73-32fa0c8c69d3
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: mobile-ios
ms.devlang: objectivec
ms.topic: article
ms.date: 03/07/2017
ms.author: saeedakhter-msft
ms.openlocfilehash: e7cbe2de6e9ae3d45448cdd36292c457a0ef4887
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-b2c-sign-in-using-an-ios-application"></a>Az Azure AD B2C: Bejelentkezés használata iOS-alkalmazás

hello Microsoft identitásplatformmal például OAuth2 és az OpenID Connect nyitott szabványok használja. További fejlesztői választott egy megnyitott szabványos protokollt használó kínál, a szalagtár toointegrate a szolgáltatások kiválasztása. Nyújtunk a forgatókönyv, míg mások például azt tooaid fejlesztők toohello Microsoft Identity platform csatlakozó alkalmazások írásához. A legtöbb tárak, amelyek megvalósítják az [hello RFC6749 OAuth2 spec](https://tools.ietf.org/html/rfc6749) képes tooconnect toohello Microsoft Identity platform vannak.

> [!WARNING]
> A Microsoft nem biztosít a külső szalagtárak javítások, és nem végrehajtva a tárak áttekintése. Ez a minta, amely tesztelték AppAuth nevű alapszintű forgatókönyvekben kompatibilisek-e az Azure AD B2C hello külső szalagtárat használ. Problémákat és funkciókra vonatkozó kérések irányított toohello könyvtár nyílt forráskódú projekt kell lennie. További információkért tekintse meg [ezt a cikket](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-libraries).
>
>

Ha új tooOAuth2 vagy az OpenID Connect, ez a minta konfigurálási nem tehetik sok logika tooyou. Javasoljuk, hogy megnézi rövid [azt korábban itt dokumentált lehetőségektől hello protokoll – áttekintés](active-directory-b2c-reference-protocols.md).

## <a name="get-an-azure-ad-b2c-directory"></a>Az Azure AD B2C-címtár beszerzése
Ahhoz, hogy használni tudja az Azure AD B2C-t, előbb létre kell hoznia egy címtárat vagy bérlőt. A könyvtár egy olyan tároló, a felhasználók, alkalmazások, csoportok és több. Ha még nem tette meg, [hozzon létre most egy B2C-címtárat](active-directory-b2c-get-started.md), mielőtt továbblépne.

## <a name="create-an-application"></a>Alkalmazás létrehozása
A következő lépésben toocreate egy alkalmazást a B2C-címtárban. hello app regisztrációs biztosítja, hogy kell-e az alkalmazással biztonságosan toocommunicate az Azure AD-információkat. Kövesse a mobilalkalmazások toocreate [ezeket az utasításokat](active-directory-b2c-app-registration.md). Ügyeljen arra, hogy:

* Tartalmaznak egy **natív ügyfél** hello alkalmazásban.
* Másolás hello **Alkalmazásazonosító** , amely hozzárendelt tooyour alkalmazást. A GUID később szüksége.
* Állítson be egy **átirányítási URI-** egy egyéni séma (például com.onmicrosoft.fabrikamb2c.exampleapp://oauth/redirect). Ezt az URI később szüksége.

[!INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Szabályzatok létrehozása
Az Azure AD B2C-ben a felhasználói élményeket [szabályzatok](active-directory-b2c-reference-policies.md) határozzák meg. Az alkalmazás tartalmaz egy identitás-élmény: egy kombinált bejelentkezési és regisztrációs. A házirend létrehozásához lásd: a [házirendek áttekintésével foglalkozó cikkben](active-directory-b2c-reference-policies.md#create-a-sign-up-policy). Hello házirend létrehozásakor ügyeljen arra, hogy:

* A **regisztrációs attribútumokat**, jelölje be hello attribútum **megjelenített név**.  Kiválaszthatja, valamint egyéb attribútumai.
* A **alkalmazási jogcímet**, hello jogcímek kiválasztására **megjelenített név** és **felhasználó Objektumazonosító**. Kiválaszthatja, hogy más jogcímeket is.
* Másolás hello **neve** egyes házirendek létrehozása után. A házirend neve a következő előtaggal van `b2c_1_` hello házirend mentésekor.  Később szüksége hello házirend nevét.

[!INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Miután létrehozta a házirendeket, készen áll a toobuild Ön az alkalmazást.

## <a name="download-hello-sample-code"></a>Hello mintakód letöltése
Egy Azure AD B2C AppAuth használó minta adtunk [a Githubon](https://github.com/Azure-Samples/active-directory-ios-native-appauth-b2c). Töltse le a hello kódot, és futtassa. a bérlői a saját Azure AD B2C toouse, hello hello utasításait követve [README.md](https://github.com/Azure-Samples/active-directory-ios-native-appauth-b2c/blob/master/README.md).

Ez a minta hozta létre hello információs utasításai szerint hello [iOS AppAuth-projektre a Githubon](https://github.com/openid/AppAuth-iOS). Hello mintát és hello szalagtár működése a további részletekért lásd hello AppAuth fontos a Githubon.

## <a name="modifying-your-app-toouse-azure-ad-b2c-with-appauth"></a>Az alkalmazás toouse Azure AD B2C-vel rendelkező AppAuth módosítása

> [!NOTE]
> AppAuth támogatja az iOS 7-es vagy újabb verzió.  Azonban azt toosupport közösségi bejelentkezések a Google, SFSafariViewController van szükség, amelyhez iOS 9-es vagy újabb rendszer szükséges.
>

### <a name="configuration"></a>Konfiguráció

Az Azure AD B2C kommunikációs megadva hello engedélyezési végpont és a token-végpont URI-azonosítók konfigurálható.  toogenerate az URI-k kell hello a következő információkat:
* Bérlő azonosítója (például contoso.onmicrosoft.com)
* Házirend neve (például B2C\_1\_SignUpIn)

hello token-végpont URI generálhatók tartományvezérlőkkel történő lecserélésével hello bérlői\_azonosítója és hello házirend\_neve a következő URL-cím hello:

```objc
static NSString *const tokenEndpoint = @"https://login.microsoftonline.com/te/<Tenant_ID>/<Policy_Name>/oauth2/v2.0/token";
```

hello engedélyezési végpont URI generálhatók tartományvezérlőkkel történő lecserélésével hello bérlői\_azonosítója és hello házirend\_neve a következő URL-cím hello:

```objc
static NSString *const authorizationEndpoint = @"https://login.microsoftonline.com/te/<Tenant_ID>/<Policy_Name>/oauth2/v2.0/authorize";
```

Futtassa a következő kód toocreate hello a AuthorizationServiceConfiguration objektum:

```objc
OIDServiceConfiguration *configuration = 
    [[OIDServiceConfiguration alloc] initWithAuthorizationEndpoint:authorizationEndpoint tokenEndpoint:tokenEndpoint];
// now we are ready tooperform hello auth request...
```

### <a name="authorizing"></a>Engedélyezése

Konfigurálja, vagy egy engedélyezési szolgáltatás konfigurációjának beolvasása után egy engedélyezési-kérelem lehet létrehozni. kérelem toocreate hello, a következő információk hello van szüksége:  
* Ügyfél-azonosító (például 00000000-0000-0000-0000-000000000000)
* Átirányítási URI egy egyéni séma (például com.onmicrosoft.fabrikamb2c.exampleapp://oauth/redirect)

Mindkét elemeket kell mentett Ha Ön volt [regisztrálja az alkalmazást](#create-an-application).

```objc
OIDAuthorizationRequest *request = 
    [[OIDAuthorizationRequest alloc] initWithConfiguration:configuration
                                                  clientId:kClientId
                                                    scopes:@[OIDScopeOpenID, OIDScopeProfile]
                                               redirectURL:[NSURL URLWithString:kRedirectUri]
                                              responseType:OIDResponseTypeCode
                                      additionalParameters:nil];

AppDelegate *appDelegate = (AppDelegate *)[UIApplication sharedApplication].delegate;
appDelegate.currentAuthorizationFlow = 
    [OIDAuthState authStateByPresentingAuthorizationRequest:request
                                   presentingViewController:self
                                                   callback:^(OIDAuthState *_Nullable authState, NSError *_Nullable error) {
        if (authState) {
            NSLog(@"Got authorization tokens. Access token: %@", authState.lastTokenResponse.accessToken);
            [self setAuthState:authState];
        } else {
            NSLog(@"Authorization error: %@", [error localizedDescription]);
            [self setAuthState:nil];
        }
    }];
```

tooset be az alkalmazás toohandle hello átirányítási toohello URI hello egyéni séma, szükséges "URL-sémákat" tooupdate hello listája az Info.pList-ben:
* Nyissa meg az Info.pList-ben.
* Például a "Csomag az operációs rendszer típusa Code" sor mutat, majd kattintson a hello \+ szimbólum.
* Nevezze át a hello új sor "URL-cím types".
* Kattintson a hello toohello balra nyíl "URL-cím types" tooopen hello fa.
* Kattintson a hello toohello balra nyíl tooopen hello fa "Konfigurációelem-0".
* Nevezze át az első elem alatt elem 0 too'URL rendszerek.
* Kattintson a hello toohello balra nyíl "URL-sémákat" tooopen hello fa.
* Van egy üres mező toohello bal oldalán hello "Érték" oszlop, "0 elem" "URL-sémákat" alatt.  Állítsa be a hello érték tooyour alkalmazás egyedi séma.  hello értéket meg kell egyeznie a redirectURL hello OIDAuthorizationRequest objektum létrehozásakor használt hello séma.  A mintában szereplő hello séma "com.onmicrosoft.fabrikamb2c.exampleapp" használtuk.

Tekintse meg a toohello [AppAuth útmutató](https://openid.github.io/AppAuth-iOS/) a hogyan toocomplete hello ki hello folyamat. Ha tooquickly kell egy működő alkalmazást az első lépései, tekintse meg [a minta](https://github.com/Azure-Samples/active-directory-ios-native-appauth-b2c). Hello kövesse hello [README.md](https://github.com/Azure-Samples/active-directory-ios-native-appauth-b2c/blob/master/README.md) tooenter a saját Azure AD B2C-konfiguráció.

Azt a rendszer mindig megnyitott toofeedback és javaslatok! Ha bármilyen nehézségbe ütközik az ebben a témakörben, vagy az ehhez a tartalomhoz javítására állnak, azt fogadjuk visszajelzéseit hello lap hello alján. A szolgáltatás kéréseket, vegye fel őket túl[UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c).
