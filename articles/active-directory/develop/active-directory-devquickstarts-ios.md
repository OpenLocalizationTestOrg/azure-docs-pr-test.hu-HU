---
title: "az iOS-alkalmazások az Azure AD aaaIntegrate |} Microsoft Docs"
description: "Hogyan toobuild, amely az Azure AD bejelentkezési és a hívások Azure AD számára az iOS-alkalmazás OAuth használatával védett API-k."
services: active-directory
documentationcenter: ios
author: brandwe
manager: mbaldwin
editor: 
ms.assetid: 42303177-9566-48ed-8abb-279fcf1e6ddb
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 01/07/2017
ms.author: brandwe
ms.custom: aaddev
ms.openlocfilehash: 6e05745b2b2b122995dcba896ab0f2ed32509e3a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="integrate-azure-ad-into-an-ios-app"></a>Az Azure AD integrálása az iOS-alkalmazás
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

> [!TIP]
> Próbálja meg az új hello előnézete [fejlesztői portálján](https://identity.microsoft.com/Docs/iOS) , amely segít, amelyekből megismerheti az Azure Active Directoryval csak néhány perc múlva!  hello fejlesztői portálján végigvezeti hello regisztrálja az alkalmazást, és az Azure AD integrálása a kódot.  Amikor elkészült, akkor kell egy egyszerű alkalmazást, amely képes hitelesíteni a felhasználók számára a bérlő és a háttérkiszolgáló fogadni és-ellenőrzéshez. 
> 
> 

Azure Active Directory (Azure AD) biztosít hello Active Directory Authentication Library vagy adal-t, iOS-ügyfelek számára, amelyek tooaccess védett erőforrásokhoz. Adal-t, hogy alkalmazása használja-e tooobtain hozzáférési jogkivonatok hello folyamat egyszerűbbé teszi. milyen egyszerűen van, ez a cikk toodemonstrate Objective C feladatlista alkalmazás létrehozásához azt:

* Lekérdezi hozzáférési jogkivonatainak hello Azure AD Graph API felület meghívásakor hello segítségével [OAuth 2.0 hitelesítési protokoll](https://msdn.microsoft.com/library/azure/dn645545.aspx).
* Egy könyvtárat a felhasználók egy adott aliasnév keres.

toobuild hello teljes működő alkalmazást kell:

1. Az alkalmazás regisztrálása az Azure ad-val.
2. Telepítse és konfigurálja az adal-t.
3. Használja az Azure AD ADAL tooget jogkivonatokat.

elindult, tooget [hello app vázat letöltése](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/skeleton.zip) vagy [letöltése befejeződött hello minta](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/complete.zip). Akkor is, amelyben felhasználók létrehozása és egy alkalmazás regisztrálása az Azure AD-bérlő kell. Ha még nem rendelkezik a bérlő [megtudhatja, hogyan egy tooget](active-directory-howto-tenant.md).


> [!TIP]
> Próbálja meg az új hello előnézete [fejlesztői portálján](https://identity.microsoft.com/Docs/iOS) , amely segít, amelyekből megismerheti az Azure AD csak néhány perc múlva. hello fejlesztői portálján végigvezeti hello regisztrálja az alkalmazást, és az Azure AD integrálása a kódot. Ha végzett, konfigurálnia kell egy egyszerű alkalmazást, amely képes hitelesíteni a felhasználók az Ön bérlőjében, és egy háttérhálózatként, hogy fogadni, és -ellenőrzéshez. 
> 
> 

## <a name="1-determine-what-your-redirect-uri-is-for-ios"></a>1. Határozza meg, milyen az átirányítási URI megadása iOS-hez
toosecurely bizonyos SSO forgatókönyvekben indítsa el az alkalmazásokat, és létre kell hoznia egy *átirányítási URI* adott formátumban. Egy átirányítási URI megadása, amelyek jogkivonatok visszatérési toohello megfelelő alkalmazás számukra feltett hello használt tooensure.


hello iOS egy átirányítási URI formátuma:

```
<app-scheme>://<bundle-id>
```

* **alkalmazás-séma** -ez regisztrálva van az XCode-projektjéhez. Fontos, hogy milyen más alkalmazások hívhatják meg. Ez az Info.plist -> található URL-cím típusú -> URL-cím azonosítója. Ha még nem rendelkezik legalább egy konfigurálni kell létrehoznia egyet.
* **Csomagazonosító** -Ez az hello Bundle Identifier "azonosító" alatt található megszünteti a projektbeállításokat, az xcode-ban.

A kód gyors üzembe helyezési példa: ***msquickstart://com.microsoft.azureactivedirectory.samples.graph.QuickStart***

## <a name="2-register-hello-directorysearcher-application"></a>2. Hello DirectorySearcher alkalmazás regisztrálása
tooset be az alkalmazás tooget jogkivonatokat, először tooregister azt az Azure ad bérlői, és adja meg az engedélyt tooaccess hello Azure AD Graph API:

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. Hello felső sávon kattintson a fiókját. A hello **Directory** menüben válassza ki a kívánt tooregister hello Active Directory-bérlőt az alkalmazást.
3. Kattintson a **több szolgáltatások** a hello bal oldali navigációs panelen, majd válassza ki **Azure Active Directory**.
4. Kattintson a **App regisztrációk**, majd válassza ki **Hozzáadás**.
5. Hajtsa végre a hello kérni fogja az új toocreate **natív ügyfélalkalmazás**.
  * Hello **neve** hello az alkalmazás az alkalmazás tooend felhasználók ismerteti.
  * Hello **átirányítási Uri-** , hogy az Azure AD által használt tooreturn token válaszok protokollt és a karakterlánc kombinációját.  Adjon meg egy értéket, amely adott tooyour alkalmazás, és hello előző átirányítási URI információkon alapul.
6. Hello regisztráció befejezését követően az Azure AD rendeli hozzá az alkalmazás egy egyedi azonosítót.  Ez az érték kell a következő szakaszok hello, ezért másolja hello alkalmazás lapján.
7. A hello **beállítások** lapon jelölje be **szükséges engedélyek** , és válassza **Hozzáadás**. Válassza ki **Microsoft Graph** hello API-t, majd adja hozzá a hello **címtáradatok olvasása** engedélyt a **delegált engedélyek**.  Az alkalmazás tooquery hello Azure AD Graph API a felhasználók állít be.

## <a name="3-install-and-configure-adal"></a>3. Telepítse és konfigurálja az adal-t
Most, hogy az Azure AD-alkalmazás, telepítse az adal-t, és az identitás-kapcsolódó kód írása.  Az Azure ad-val ADAL toocommunicate tooprovide kell azt bizonyos információkat az alkalmazás regisztrációját.

1. Kezdje az ADAL toohello DirectorySearcher projekt hozzáadása a CocoaPods segítségével.

    ```
    $ vi Podfile
    ```
2. Adja hozzá a következő toothis podfile hello:

    ```
    source 'https://github.com/CocoaPods/Specs.git'
    link_with ['QuickStart']
    xcodeproj 'QuickStart'

    pod 'ADALiOS'
    ```

3. Most töltse be hello podfile CocoaPods segítségével. Ebben a lépésben létrehoz egy új XCode-munkaterületet, hogy betöltve.

    ```
    $ pod install
    ...
    $ open QuickStart.xcworkspace
    ```

4. Hello Gyorsútmutató-projekt, nyissa meg hello plist-fájlt `settings.plist`.  Hello értékek hello elemeinek hello szakasz tooreflect hello értékek hello Azure-portálon megadott helyett. A kód hivatkozik ezeket az értékeket, minden alkalommal adal-t.
  * Hello `tenant` hello tartománya, akkor az Azure AD-bérlőn, például a contoso.onmicrosoft.com.
  * Hello `clientId` hello ügyfél-azonosító az alkalmazás hello portálról másolt.
  * Hello `redirectUri` hello átirányítási URL-cím hello portálon regisztrált.

## <a name="4----use-adal-tooget-tokens-from-azure-ad"></a>4.    Az Azure AD ADAL tooget jogkivonatok használata
hello alapelv ADAL mögött, hogy a hozzáférési tokent kell, ha egyszerűen meghívja a completionBlock `+(void) getToken : `, és ADAL rest hello.  

1. A hello `QuickStart` projektben nyissa meg `GraphAPICaller.m` , és keresse meg a hello `// TODO: getToken for generic Web API flows. Returns a token with no additional parameters provided.` Megjegyzés hello felső közelében.  Ez az adott ADAL hello koordináták CompletionBlock, az Azure ad-val, toocommunicate keresztül továbbítja, és mondja el hogyan toocache jogkivonatokat.

    ```ObjC
    +(void) getToken : (BOOL) clearCache
               parent:(UIViewController*) parent
    completionHandler:(void (^) (NSString*, NSError*))completionBlock;
    {
        AppData* data = [AppData getInstance];
        if(data.userItem){
            completionBlock(data.userItem.accessToken, nil);
            return;
        }

        ADAuthenticationError *error;
        authContext = [ADAuthenticationContext authenticationContextWithAuthority:data.authority error:&error];
        authContext.parentController = parent;
        NSURL *redirectUri = [[NSURL alloc]initWithString:data.redirectUriString];

        [ADAuthenticationSettings sharedInstance].enableFullScreen = YES;
        [authContext acquireTokenWithResource:data.resourceId
                                     clientId:data.clientId
                                  redirectUri:redirectUri
                               promptBehavior:AD_PROMPT_AUTO
                                       userId:data.userItem.userInformation.userId
                        extraQueryParameters: @"nux=1" // if this strikes you as strange it was legacy toodisplay hello correct mobile UX. You most likely won't need it in your code.
                             completionBlock:^(ADAuthenticationResult *result) {

                                  if (result.status != AD_SUCCEEDED)
                                  {
                                     completionBlock(nil, result.error);
                                  }
                                  else
                                  {
                                      data.userItem = result.tokenCacheStoreItem;
                                      completionBlock(result.tokenCacheStoreItem.accessToken, nil);
                                  }
                             }];
    }

    ```

2. Most létre kell toouse a token toosearch hello graph lévő felhasználók számára. Hello található `// TODO: implement SearchUsersList` megjegyzés. Ez a módszer egy GET kérelmet toohello Azure AD Graph API tooquery teszi a felhasználók számára, akiknek UPN keresőkifejezéssel megadott hello kezdődik.  tooquery hello Azure AD Graph API-t kell tooinclude egy access_token a hello `Authorization` hello kérelem fejlécében. Ez az adal-t honnan.

    ```ObjC
    +(void) searchUserList:(NSString*)searchString
                    parent:(UIViewController*) parent
          completionBlock:(void (^) (NSMutableArray* Users, NSError* error)) completionBlock
    {
        if (!loadedApplicationSettings)
       {
            [self readApplicationSettings];
        }
        
        AppData* data = [AppData getInstance];

        NSString *graphURL = [NSString stringWithFormat:@"%@%@/users?api-version=%@&$filter=startswith(userPrincipalName, '%@')", data.taskWebApiUrlString, data.tenant, data.apiversion, searchString];

        [self craftRequest:[self.class trimString:graphURL]
                    parent:parent
         completionHandler:^(NSMutableURLRequest *request, NSError *error) {

             if (error != nil)
             {
                 completionBlock(nil, error);
             }
             else
             {

                 NSOperationQueue *queue = [[NSOperationQueue alloc]init];

                 [NSURLConnection sendAsynchronousRequest:request queue:queue completionHandler:^(NSURLResponse *response, NSData *data, NSError *error) {

                     if (error == nil && data != nil){

                         NSDictionary *dataReturned = [NSJSONSerialization JSONObjectWithData:data options:0 error:nil];

                         // We can grab hello JSON node at hello top tooget our graph data.
                         NSArray *graphDataArray = [dataReturned objectForKey:@"value"];

                         // Don't be thrown off by hello key name being "value". It really is hello name of the
                         // first node. :-)

                         // Each object is a key value pair
                         NSDictionary *keyValuePairs;
                         NSMutableArray* Users = [[NSMutableArray alloc]init];

                         for(int i =0; i < graphDataArray.count; i++)
                         {
                             keyValuePairs = [graphDataArray objectAtIndex:i];

                             User *s = [[User alloc]init];
                             s.upn = [keyValuePairs valueForKey:@"userPrincipalName"];
                             s.name =[keyValuePairs valueForKey:@"givenName"];

                             [Users addObject:s];
                         }

                         completionBlock(Users, nil);
                     }
                     else
                     {
                         completionBlock(nil, error);
                     }

                }];
             }
         }];

    }

    ```


3. Ha az alkalmazás kísérel meg jogkivonat meghívásával `getToken(...)`, ADAL tooreturn jogkivonat kísérletek hello felhasználói hitelesítő adatok kérése nélkül.  Ha ADAL hello felhasználó van szüksége a tooget jogkivonat toosign, azt fogja bejelentkezésnél párbeszédpanel megjelenítése, hello felhasználói hitelesítő adatok összegyűjtése és sikeres hitelesítés után térjen vissza a jogkivonat.  Ha az adal TÁRAT nem képes tooreturn jogkivonat bármilyen okból, jelez egy `AdalException`.

> [!Note] 
> Hello `AuthenticationResult` objektum tartalmaz egy `tokenCacheStoreItem` objektum, amely lehet, hogy az alkalmazás esetleg használt toocollect hello információkat. A gyors üzembe helyezés, hello `tokenCacheStoreItem` használt toodetermine akkor, ha a hitelesítés már megtörtént.
>
>

## <a name="5-build-and-run-hello-application"></a>5. Hozza létre és hello alkalmazás futtatása
Gratulálunk! Most már rendelkezik egy működő iOS-alkalmazást, amely hitelesíti a felhasználókat, biztonságos webes API-k hívására OAuth 2.0-s, és alapszintű hello felhasználó adatainak beolvasása.  Ha még nem tette meg, most az hello idő toopopulate a bérlő az egyes felhasználók.  Indítsa el a gyorsindító alkalmazást, és jelentkezzen be valamelyik azoknak a felhasználóknak.  Az egyszerű Felhasználónevük alapján más felhasználók kereséséhez.  Hello-alkalmazások bezárása, majd indítsa el újból.  Figyelje meg, hogy hello felhasználói munkamenet érintetlen marad.

ADAL teszi, hogy könnyen tooincorporate mindezeket a funkciókat az alkalmazásba közös identitás.  Az gondoskodik összes hello dirty munkahelyi meg, mint a gyorsítótár-felügyelet OAuth protokoll támogatása, és a felhasználói felület toosign a hello felhasználói bemutató, és lejárt jogkivonatok frissítésekor.  Valóban tooknow szüksége egy egyetlen API-hívással `getToken`.

A megadott befejeződött hello mintát (a konfigurációs értékek nélkül) referenciaként [GitHub](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/complete.zip).  

## <a name="next-steps"></a>Következő lépések
Most már továbbléphet tooadditional forgatókönyvek.  Érdemes lehet tootry:

* [Egy Node.JS webes API-t az Azure ad-vel biztonságos](active-directory-devquickstarts-webapi-nodejs.md)
* Ismerje meg, [hogyan tooenable adal-t használó IOS alkalmazások közötti SSO](active-directory-sso-ios.md)  

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]

