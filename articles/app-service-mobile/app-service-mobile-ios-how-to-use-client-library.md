---
title: "aaaHow tooUse iOS SDK Azure Mobile Apps-alkalmazáshoz"
description: "Hogyan tooUse iOS SDK Azure Mobile Apps-alkalmazáshoz"
services: app-service\mobile
documentationcenter: ios
author: ysxu
manager: yochayk
editor: 
ms.assetid: 4e8e45df-c36a-4a60-9ad4-393ec10b7eb9
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 10/01/2016
ms.author: yuaxu
ms.openlocfilehash: fa299ab3f152bad12d821832fa9fb5495d1fa296
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-ios-client-library-for-azure-mobile-apps"></a>Hogyan tooUse iOS Azure Mobile Apps készült ügyféloldali kódtára
[!INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

Ez az útmutató útmutatást ad, hogy tooperform szolgáltatást használó általános forgatókönyvhöz hello legújabb [Azure Mobile Apps iOS SDK][1]. Ha új tooAzure Mobile Apps, először végezzen [Azure Mobile Apps gyors üzembe helyezés] toocreate egy háttér, hozzon létre egy táblát, és egy előre elkészített iOS Xcode-projekt letöltése. Az útmutató azt összpontosítani hello ügyféloldali iOS SDK is. toolearn arról kiszolgálóoldali SDK hello háttérkiszolgáló hello című hello Server SDK HOWTOs.

## <a name="reference-documentation"></a>Segédanyagok
hello hello iOS ügyfél SDK referenciadokumentációt tartalmaz a következő helyen található: [Azure Mobile Apps iOS ügyfél hivatkozási][2].

## <a name="supported-platforms"></a>A támogatott platformok
hello iOS SDK támogatja Objective-C projektek, a Swift 2.2 projektek és a Swift 2.3-projektek az iOS 8.0-s vagy újabb verziójú.

hello "server-folyamat" hitelesítési egy webes nézet jelenik meg a felhasználói felület hello használ.  Hello eszköz nem tudja toopresent webes nézet felhasználói Felületet, majd egy másik hitelesítési módszer szükség, amely akkor hello termék külső hello hatókör.  
Ez az SDK így nem alkalmas figyelési típusú vagy hasonló módon korlátozott eszközök.

## <a name="Setup"></a>A telepítő és Előfeltételek
Ez az útmutató feltételezi, hogy létrehozott egy táblát a háttérkiszolgálón. Ez az útmutató feltételezi, hogy hello táblához hello táblák ugyanazon séma ezen oktatóprogram a. Ez az útmutató feltételezi, hogy a kódban hivatkozik `MicrosoftAzureMobile.framework` , majd importálja `MicrosoftAzureMobile/MicrosoftAzureMobile.h`.

## <a name="create-client"></a>Hogyan: ügyfél létrehozása
a projekt egy Azure Mobile Apps-háttéralkalmazás tooaccess hozzon létre egy `MSClient`. Cserélje le `AppUrl` hello alkalmazás URL-címmel. Hagyhatja `gatewayURLString` és `applicationKey` üres. Ha állít be egy átjárót a hitelesítéshez, feltöltése `gatewayURLString` hello átjáró URL-címmel.

**Objective-C**:

```
MSClient *client = [MSClient clientWithApplicationURLString:@"AppUrl"];
```

**SWIFT**:

```
let client = MSClient(applicationURLString: "AppUrl")
```


## <a name="table-reference"></a>Hogyan: táblahivatkozás létrehozása
tooaccess vagy frissítés adatokat, a hivatkozás toohello háttér tábla létrehozása. Cserélje le `TodoItem` hello nevű a táblázat

**Objective-C**:

```
MSTable *table = [client tableWithName:@"TodoItem"];
```

**SWIFT**:

```
let table = client.tableWithName("TodoItem")
```


## <a name="querying"></a>Útmutató: adatok lekérdezése
toocreate egy adatbázis-lekérdezés lekérdezés hello `MSTable` objektum. hello alábbi lekérdezés lekérdezi összes hello elemet `TodoItem` és a naplók hello minden elem szövegét.

**Objective-C**:

```
[table readWithCompletion:^(MSQueryResult *result, NSError *error) {
        if(error) { // error is nil if no error occured
                NSLog(@"ERROR %@", error);
        } else {
                for(NSDictionary *item in result.items) { // items is NSArray of records that match query
                        NSLog(@"Todo Item: %@", [item objectForKey:@"text"]);
                }
        }
}];
```

**SWIFT**:

```
table.readWithCompletion { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let items = result?.items {
        for item in items {
            print("Todo Item: ", item["text"])
        }
    }
}
```

## <a name="filtering"></a>Hogyan: szűrő adatokat adott vissza.
toofilter eredmény elérése érdekében számos lehetőség közül.

toofilter használatával predikátum, használjon egy `NSPredicate` és `readWithPredicate`. hello következő szűrők visszaadott adatok toofind csak hiányos Todo elemeket.

**Objective-C**:

```
// Create a predicate that finds items where complete is false
NSPredicate * predicate = [NSPredicate predicateWithFormat:@"complete == NO"];
// Query hello TodoItem table
[table readWithPredicate:predicate completion:^(MSQueryResult *result, NSError *error) {
        if(error) {
                NSLog(@"ERROR %@", error);
        } else {
                for(NSDictionary *item in result.items) {
                        NSLog(@"Todo Item: %@", [item objectForKey:@"text"]);
                }
        }
}];
```

**SWIFT**:

```
// Create a predicate that finds items where complete is false
let predicate =  NSPredicate(format: "complete == NO")
// Query hello TodoItem table
table.readWithPredicate(predicate) { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let items = result?.items {
        for item in items {
            print("Todo Item: ", item["text"])
        }
    }
}
```

## <a name="query-object"></a>Hogyan: MSQuery használata
tooperform egy összetett lekérdezés (beleértve a rendezés és lapozáshoz), hozzon létre egy `MSQuery` objektum, közvetlenül vagy predikátum használatával:

**Objective-C**:

```
MSQuery *query = [table query];
MSQuery *query = [table queryWithPredicate: [NSPredicate predicateWithFormat:@"complete == NO"]];
```

**SWIFT**:

```
let query = table.query()
let query = table.queryWithPredicate(NSPredicate(format: "complete == NO"))
```

`MSQuery`lehetővé teszi, hogy több lekérdezés viselkedések szabályozhatja.

* Adja meg az eredmények sorrendje
* Mely mezők tooreturn korlátozása
* Tooreturn rekordok számának korlátozása
* Adja meg a számuk adott válasz
* Adja meg az egyéni lekérdezési karakterlánc paramétert kérelem
* További funkciók alkalmazása

Hajtsa végre egy `MSQuery` meghívásával lekérdezés `readWithCompletion` hello objektumon.

## <a name="sorting"></a>Hogyan: MSQuery az adatok rendezése
toosort eredményeket, például vizsgáljuk meg. mező "text" növekvő, azután a "teljes" csökkenő toosort meghívása `MSQuery` , például így:

**Objective-C**:

```
[query orderByAscending:@"text"];
[query orderByDescending:@"complete"];
[query readWithCompletion:^(MSQueryResult *result, NSError *error) {
        if(error) {
                NSLog(@"ERROR %@", error);
        } else {
                for(NSDictionary *item in result.items) {
                        NSLog(@"Todo Item: %@", [item objectForKey:@"text"]);
                }
        }
}];
```

**SWIFT**:

```
query.orderByAscending("text")
query.orderByDescending("complete")
query.readWithCompletion { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let items = result?.items {
        for item in items {
            print("Todo Item: ", item["text"])
        }
    }
}
```


## <a name="selecting"></a><a name="parameters"></a>Hogyan: mezők korlátjának növelését, és bontsa ki a lekérdezési karakterlánc paramétereket MSQuery
a lekérdezés által visszaadott toolimit mezők toobe hello hello mezők nevét adja meg hello **selectFields** tulajdonság. Ebben a példában csak a hello szöveg és a befejezett mezők adja vissza:

**Objective-C**:

```
query.selectFields = @[@"text", @"complete"];
```

**SWIFT**:

```
query.selectFields = ["text", "complete"]
```

hello server tooinclude további lekérdezési karakterlánc-paraméterrel (például azért, mert egy egyéni kiszolgálóoldali parancsfájl használja őket) kérelmezéséhez feltöltése `query.parameters` , például így:

**Objective-C**:

```
query.parameters = @{
    @"myKey1" : @"value1",
    @"myKey2" : @"value2",
};
```

**SWIFT**:

```
query.parameters = ["myKey1": "value1", "myKey2": "value2"]
```

## <a name="paging"></a>Hogyan: oldalméret konfigurálása
Az Azure Mobile Apps hello mérete Lapvezérlők hello hello háttér táblázatokból egyszerre vannak lekért rekordok száma. A hívás túl`pull` adatokat szeretné majd a batch-adatokat, a lap mérete alapján, amíg nincs további rekordok toopull.

A lap méret használatával lehetséges tooconfigure **MSPullSettings** alább látható módon. hello alapértelmezett oldal mérete 50, és az alábbi példa hello too3 módosítja azt.

Konfigurálhatja a különböző méretet a jobb teljesítmény érdekében. Ha kis rekordok nagy száma, a magas oldalméret csökkenti server üzenetváltások hello számát.

Ez a beállítás csak hello oldalméret hello ügyféloldalon szabályozza. Ha hello ügyfél hello Mobile Apps-háttéralkalmazás támogatja, mint nagyobb oldalméretet kér, hello oldalméret tárfiókonként maximum hello maximális hello háttér pedig konfigurált toosupport.

Ez a beállítás akkor is hello *szám* az rekordok, nem hello *bájtméretnek*.

Ha növeli hello ügyfél oldalméret, is növelje hello oldalméret hello kiszolgálón. Lásd: ["hogyan: hello tábla a lapozófájl méretének módosítása"](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) a hello lépések toodo ez.

**Objective-C**:

```
  MSPullSettings *pullSettings = [[MSPullSettings alloc] initWithPageSize:3];
  [table  pullWithQuery:query queryId:@nil settings:pullSettings
                        completion:^(NSError * _Nullable error) {
                               if(error) {
                    NSLog(@"ERROR %@", error);
                }
                           }];
```


**SWIFT**:

```
let pullSettings = MSPullSettings(pageSize: 3)
table.pullWithQuery(query, queryId:nil, settings: pullSettings) { (error) in
    if let err = error {
        print("ERROR ", err)
    }
}
```

## <a name="inserting"></a>Útmutató: adatok beszúrása
tooinsert egy új sorának, hozzon létre egy `NSDictionary` és invoke `table insert`. Ha [dinamikus séma] van engedélyezve van, hello Azure App Service mobil-háttéralkalmazást automatikusan hoz létre új oszlopok hello alapján `NSDictionary`.

Ha `id` van a nem Microsofttól származó, hello háttér automatikusan hoz létre egy új egyedi azonosítót. Adja meg a saját `id` toouse e-mail címeket, a felhasználónevek vagy a saját egyéni értékeket, azonosítóját. Saját azonosító megadása is megkönnyítik illesztések és üzleti célú adatbázis programot.

Hello `result` beszúrt hello új cikk tartalmazza. Attól függően, hogy a kiszolgáló logika lehet további, vagy módosított adatok képest toowhat toohello server lett átadva.

**Objective-C**:

```
NSDictionary *newItem = @{@"id": @"custom-id", @"text": @"my new item", @"complete" : @NO};
[table insert:newItem completion:^(NSDictionary *result, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item: %@", [result objectForKey:@"text"]);
    }
}];
```

**SWIFT**:

```
let newItem = ["id": "custom-id", "text": "my new item", "complete": false]
table.insert(newItem) { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let item = result {
        print("Todo Item: ", item["text"])
    }
}
```

## <a name="modifying"></a>Útmutató: adatok módosítása
egy meglévő sor tooupdate módosítani egy elemet, és a hívás `update`:

**Objective-C**:

```
NSMutableDictionary *newItem = [oldItem mutableCopy]; // oldItem is NSDictionary
[newItem setValue:@"Updated text" forKey:@"text"];
[table update:newItem completion:^(NSDictionary *result, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item: %@", [result objectForKey:@"text"]);
    }
}];
```

**SWIFT**:

```
if let newItem = oldItem.mutableCopy() as? NSMutableDictionary {
    newItem["text"] = "Updated text"
    table2.update(newItem as [NSObject: AnyObject], completion: { (result, error) -> Void in
        if let err = error {
            print("ERROR ", err)
        } else if let item = result {
            print("Todo Item: ", item["text"])
        }
    })
}
```

Másik lehetőségként adja meg a hello Sorazonosító és a frissített hello mező:

**Objective-C**:

```
[table update:@{@"id":@"custom-id", @"text":"my EDITED item"} completion:^(NSDictionary *result, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item: %@", [result objectForKey:@"text"]);
    }
}];
```

**SWIFT**:

```
table.update(["id": "custom-id", "text": "my EDITED item"]) { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let item = result {
        print("Todo Item: ", item["text"])
    }
}
```

Legalább hello `id` attribútumot úgy kell beállítani, amikor frissíteni.

## <a name="deleting"></a>Útmutató: adatok törlése
egy elem, toodelete meghívása `delete` hello elemhez:

**Objective-C**:

```
[table delete:item completion:^(id itemId, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item ID: %@", itemId);
    }
}];
```

**SWIFT**:

```
table.delete(newItem as [NSObject: AnyObject]) { (itemId, error) in
    if let err = error {
        print("ERROR ", err)
    } else {
        print("Todo Item ID: ", itemId)
    }
}
```

Azt is megteheti törölje a Sorazonosító megadásával:

**Objective-C**:

```
[table deleteWithId:@"37BBF396-11F0-4B39-85C8-B319C729AF6D" completion:^(id itemId, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item ID: %@", itemId);
    }
}];
```

**SWIFT**:

```
table.deleteWithId("37BBF396-11F0-4B39-85C8-B319C729AF6D") { (itemId, error) in
    if let err = error {
        print("ERROR ", err)
    } else {
        print("Todo Item ID: ", itemId)
    }
}
```

Legalább hello `id` attribútumot úgy kell beállítani, ha törli az elvégzése.

## <a name="customapi"></a>Útmutató: egyéni API hívása
Egy egyéni API-val olyan háttér funkciót is elérhetővé teheti. Nincs beállítva a toomap tooa művelet. Nem csak akkor kapnak az üzenetkezelési teljesebb körű vezérlése, még akkor is is fejlécek olvasási vagy állították be, és módosítsa a hello válasz törzsében formátuma. Hogyan toocreate egyéni API hello háttérkiszolgálón, olvassa el toolearn [egyéni API-k](app-service-mobile-node-backend-how-to-use-server-sdk.md#work-easy-apis)

toocall egy egyéni API-hívás `MSClient.invokeAPI`. hello kérelem-válasz tartalom JSON tekintendők. toouse más adathordozók típusairól [használja más túlterhelése hello `invokeAPI` ] [ 5].  toomake egy `GET` ahelyett, hogy kérjen egy `POST` kérelmezéséhez set paraméter `HTTPMethod` túl`"GET"` és paraméter `body` túl`nil` (mivel a GET-kérésekhez nincs az üzenet törzse.) Ha az egyéni API támogatja az egyéb HTTP-műveletek, módosítsa `HTTPMethod` megfelelően.

**Objective-C**:

```
[self.client invokeAPI:@"sendEmail"
                  body:@{ @"contents": @"Hello world!" }
            HTTPMethod:@"POST"
            parameters:@{ @"to": @"bill@contoso.com", @"subject" : @"Hi!" }
               headers:nil
            completion: ^(NSData *result, NSHTTPURLResponse *response, NSError *error) {
                if(error) {
                    NSLog(@"ERROR %@", error);
                } else {
                    // Do something with result
                }
            }];
```

**SWIFT**:

```
client.invokeAPI("sendEmail",
            body: [ "contents": "Hello World" ],
            HTTPMethod: "POST",
            parameters: [ "to": "bill@contoso.com", "subject" : "Hi!" ],
            headers: nil)
            {
                (result, response, error) -> Void in
                if let err = error {
                    print("ERROR ", err)
                } else if let res = result {
                          // Do something with result
                }
        }
```

## <a name="templates"></a>Útmutató: regisztráció leküldéses sablonok toosend platformfüggetlen értesítések
tooregister sablonok adja át a sablon is van a **client.push registerDeviceToken** ügyfélalkalmazás metódust.

**Objective-C**:

```
[client.push registerDeviceToken:deviceToken template:iOSTemplate completion:^(NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    }
}];
```

**SWIFT**:

```
    client.push?.registerDeviceToken(NSData(), template: iOSTemplate, completion: { (error) in
        if let err = error {
            print("ERROR ", err)
        }
    })
```

A sablonok NSDictionary típusú, és tartalmazhat több sablonok hello a következő formátumban:

**Objective-C**:

```
NSDictionary *iOSTemplate = @{ @"templateName": @{ @"body": @{ @"aps": @{ @"alert": @"$(message)" } } } };
```

**SWIFT**:

```
let iOSTemplate = ["templateName": ["body": ["aps": ["alert": "$(message)"]]]]
```

Minden címkék biztonsági hello kérelem üres karaktert törölni.  tooadd címkéket tooinstallations vagy sablonok telepítések belül, lásd: [használható hello .NET háttérkiszolgáló SDK az Azure Mobile Apps a][4].  regisztrált ezeket a sablonokat, toosend értesítéseket együttműködve [Notification Hubs API-k][3].

## <a name="errors"></a>Hogyan: hibák kezelésének
Az Azure App Service mobil-háttéralkalmazást hívásakor hello befejezési blokk tartalmaz egy `NSError` paraméter. Ha hiba lép fel, a paraméter nem üres. A kódban Ez a paraméter ellenőrizze és hello hiba szükséges, kezelését, ahogyan az kódrészleteket megelőző hello.

hello fájl [ `<WindowsAzureMobileServices/MSError.h>` ] [ 6] hello állandók meghatározása `MSErrorResponseKey`, `MSErrorRequestKey`, és `MSErrorServerItemKey`. tooget kapcsolódó további adatok toohello hiba:

**Objective-C**:

```
NSDictionary *serverItem = [error.userInfo objectForKey:MSErrorServerItemKey];
```

**SWIFT**:

```
let serverItem = error.userInfo[MSErrorServerItemKey]
```

Ezenkívül hello fájl meghatározza, hogy minden hibakód állandókat:

**Objective-C**:

```
if (error.code == MSErrorPreconditionFailed) {
```

**SWIFT**:

```
if (error.code == MSErrorPreconditionFailed) {
```

## <a name="adal"></a>Hogyan: hitelesíti a felhasználókat az Active Directory Authentication Library hello
Az alkalmazás Azure Active Directory használatával hello Active Directory Authentication Library (ADAL) toosign felhasználók is használhatja. Ügyfél adatfolyam-hitelesítéshez az identitásszolgáltató SDK előnyösebb toousing hello `loginWithProvider:completion:` metódust.  Ügyfél folyamata hitelesítést biztosít több natív UX abba, és lehetővé teszi, hogy további testreszabási.

1. A mobil-háttéralkalmazás számára az AAD-bejelentkezés konfigurálása következő hello [hogyan tooconfigure App Service az Active Directory bejelentkezési] [ 7] oktatóanyag. Győződjön meg arról, hogy toocomplete hello opcionális lépés egy natív ügyfélalkalmazás regisztrációján. Az iOS, azt javasoljuk, hogy hello átirányítási URI megadása hello űrlap `<app-scheme>://<bundle-id>`. További információkért lásd: hello [ADAL iOS gyors üzembe helyezés][8].
2. Telepítse a Cocoapods segítségével adal-t. A következő definícióját, hogy Podfile tooinclude hello szerkesztése **YOUR-projekt** az Xcode-projektjéhez nevű hello:

        source 'https://github.com/CocoaPods/Specs.git'
        link_with ['YOUR-PROJECT']
        xcodeproj 'YOUR-PROJECT'

   és hello Pod:

        pod 'ADALiOS'
3. Hello terminálban futtatása használatával `pod install` hello könyvtárból a projektet tartalmazó, majd nyissa meg a létrehozott hello Xcode-munkaterületet (hello projekthez nem).
4. Adja hozzá a következő kód tooyour alkalmazás, használ toohello nyelv szerint hello. Minden ellenőrizze a cserét:

   * Cserélje le **INSERT-SZOLGÁLTATÓ-Itt** hello nevű hello bérlő, amelyben az alkalmazás kiépítve. A következő formátumban kell megadni https://login.microsoftonline.com/contoso.onmicrosoft.com. Ez az érték hello [klasszikus Azure portálra] Azure Active Directory tartományi lapfülén hello másolhatók.
   * Cserélje le **INSERT-erőforrás-azonosító-Itt** hello ügyfél-azonosítójú a mobil-háttéralkalmazást. Az ügyfél-azonosító szerezhet be a hello **speciális** lap **Azure Active Directory beállításai** hello portálon.
   * Cserélje le **INSERT-ügyfél-azonosító-Itt** hello ügyfél-azonosítójú hello natív ügyfélalkalmazás másolta.
   * Cserélje le **INSERT-REDIRECT-URI-Itt** a hellyel való */.auth/login/done* végpont hello HTTPS protokollt használ. Ez az érték túl hasonló legyen*https://contoso.azurewebsites.net/.auth/login/done*.

**Objective-C**:

    #import <ADALiOS/ADAuthenticationContext.h>
    #import <ADALiOS/ADAuthenticationSettings.h>
    // ...
    - (void) authenticate:(UIViewController*) parent
               completion:(void (^) (MSUser*, NSError*))completionBlock;
    {
        NSString *authority = @"INSERT-AUTHORITY-HERE";
        NSString *resourceId = @"INSERT-RESOURCE-ID-HERE";
        NSString *clientId = @"INSERT-CLIENT-ID-HERE";
        NSURL *redirectUri = [[NSURL alloc]initWithString:@"INSERT-REDIRECT-URI-HERE"];
        ADAuthenticationError *error;
        ADAuthenticationContext *authContext = [ADAuthenticationContext authenticationContextWithAuthority:authority error:&error];
        authContext.parentController = parent;
        [ADAuthenticationSettings sharedInstance].enableFullScreen = YES;
        [authContext acquireTokenWithResource:resourceId
                                     clientId:clientId
                                  redirectUri:redirectUri
                              completionBlock:^(ADAuthenticationResult *result) {
                                  if (result.status != AD_SUCCEEDED)
                                  {
                                      completionBlock(nil, result.error);;
                                  }
                                  else
                                  {
                                      NSDictionary *payload = @{
                                                                @"access_token" : result.tokenCacheStoreItem.accessToken
                                                                };
                                      [client loginWithProvider:@"aad" token:payload completion:completionBlock];
                                  }
                              }];
    }


**SWIFT**:

    // add hello following imports tooyour bridging header:
    //        #import <ADALiOS/ADAuthenticationContext.h>
    //        #import <ADALiOS/ADAuthenticationSettings.h>

    func authenticate(parent: UIViewController, completion: (MSUser?, NSError?) -> Void) {
        let authority = "INSERT-AUTHORITY-HERE"
        let resourceId = "INSERT-RESOURCE-ID-HERE"
        let clientId = "INSERT-CLIENT-ID-HERE"
        let redirectUri = NSURL(string: "INSERT-REDIRECT-URI-HERE")
        var error: AutoreleasingUnsafeMutablePointer<ADAuthenticationError?> = nil
        let authContext = ADAuthenticationContext(authority: authority, error: error)
        authContext.parentController = parent
        ADAuthenticationSettings.sharedInstance().enableFullScreen = true
        authContext.acquireTokenWithResource(resourceId, clientId: clientId, redirectUri: redirectUri) { (result) in
                if result.status != AD_SUCCEEDED {
                    completion(nil, result.error)
                }
                else {
                    let payload: [String: String] = ["access_token": result.tokenCacheStoreItem.accessToken]
                    client.loginWithProvider("aad", token: payload, completion: completion)
                }
            }
    }

## <a name="facebook-sdk"></a>Hogyan: hello Facebook SDK iOS-hez a felhasználók hitelesítéséhez
Is használhatja hello Facebook SDK iOS-toosign felhasználóknak az alkalmazás Facebook használatával.  Ügyfél-hitelesítési folyamat használata hatékonyabb toousing hello `loginWithProvider:completion:` metódust.  hello ügyfélhitelesítés folyamat több natív UX abba biztosít, és lehetővé teszi, hogy további testreszabási.

1. A mobil-háttéralkalmazás a Facebook-bejelentkezés konfigurálása a következő a [hogyan tooconfigure App Service Facebook bejelentkezési azonosítóhoz] [ 9] oktatóanyag.
2. Hello Facebook SDK az iOS által következő hello telepítése [Facebook SDK iOS - első lépések] [ 10] dokumentációját. Alkalmazás létrehozása, helyett hello iOS platform tooyour meglévő regisztrációs is hozzáadhat.
3. Facebook tartozó dokumentáció néhány Objective-C kódjának hello App delegált tartalmazza. Ha használ **Swift**, használhatja a következő AppDelegate.swift fordításainak hello:

        // Add hello following import tooyour bridging header:
        //        #import <FBSDKCoreKit/FBSDKCoreKit.h>

        func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject : AnyObject]?) -> Bool {
            FBSDKApplicationDelegate.sharedInstance().application(application, didFinishLaunchingWithOptions: launchOptions)
            // Add any custom logic here.
            return true
        }

        func application(application: UIApplication, openURL url: NSURL, sourceApplication: String?, annotation: AnyObject?) -> Bool {
            let handled = FBSDKApplicationDelegate.sharedInstance().application(application, openURL: url, sourceApplication: sourceApplication, annotation: annotation)
            // Add any custom logic here.
            return handled
        }
4. Továbbá tooadding a `FBSDKCoreKit.framework` tooyour projektre, túl is adjon hozzá egy hivatkozást`FBSDKLoginKit.framework` hello a megszokott módon.
5. Adja hozzá a következő kód tooyour alkalmazás, használ toohello nyelv szerint hello.

**Objective-C**:

    #import <FBSDKLoginKit/FBSDKLoginKit.h>
    #import <FBSDKCoreKit/FBSDKAccessToken.h>
    // ...
    - (void) authenticate:(UIViewController*) parent
               completion:(void (^) (MSUser*, NSError*)) completionBlock;
    {        
        FBSDKLoginManager *loginManager = [[FBSDKLoginManager alloc] init];
        [loginManager
         logInWithReadPermissions: @[@"public_profile"]
         fromViewController:parent
         handler:^(FBSDKLoginManagerLoginResult *result, NSError *error) {
             if (error) {
                 completionBlock(nil, error);
             } else if (result.isCancelled) {
                 completionBlock(nil, error);
             } else {
                 NSDictionary *payload = @{
                                           @"access_token":result.token.tokenString
                                           };
                 [client loginWithProvider:@"facebook" token:payload completion:completionBlock];
             }
         }];
    }

**SWIFT**:

    // Add hello following imports tooyour bridging header:
    //        #import <FBSDKLoginKit/FBSDKLoginKit.h>
    //        #import <FBSDKCoreKit/FBSDKAccessToken.h>

    func authenticate(parent: UIViewController, completion: (MSUser?, NSError?) -> Void) {
        let loginManager = FBSDKLoginManager()
        loginManager.logInWithReadPermissions(["public_profile"], fromViewController: parent) { (result, error) in
            if (error != nil) {
                completion(nil, error)
            }
            else if result.isCancelled {
                completion(nil, error)
            }
            else {
                let payload: [String: String] = ["access_token": result.token.tokenString]
                client.loginWithProvider("facebook", token: payload, completion: completion)
            }
        }
    }

## <a name="twitter-fabric"></a>Útmutató: az IOS-es Twitter-hálóval felhasználók hitelesítéséhez
Is használhatja háló iOS toosign felhasználóknak az alkalmazás Twitter használatával. Ügyfél folyamata a hitelesítés az előnyösebb toousing hello `loginWithProvider:completion:` metódus, mert több natív UX abba biztosít, és lehetővé teszi, hogy további testreszabási.

1. A mobil-háttéralkalmazás számára Twitter-bejelentkezés konfigurálása a következő hello [hogyan tooconfigure App Service Twitter bejelentkezési azonosítóhoz](app-service-mobile-how-to-configure-twitter-authentication.md) oktatóanyag.
2. Adja hozzá a háló tooyour projekt által következő hello [(iOS) – első lépések a háló] dokumentáció és TwitterKit beállítása.

   > [!NOTE]
   > Alapértelmezés szerint háló létrehoz egy Twitter-alkalmazást. Az alkalmazás létrehozása hello kulcsa és fogyasztói titkos kulcsot, korábban létrehozott alábbi kódtöredékek hello regisztrálásával elkerülheti a.    Azt is megteheti, lecserélheti hello kulcsa és fogyasztói titkos kulcs értékeket, hogy megadja a hello szolgáltatás tooApp értékekkel című hello [háló irányítópult]. Válassza ezt a beállítást, ha lehet, hogy tooset hello visszahívási URL-cím tooa helyőrző értékű, például a `https://<yoursitename>.azurewebsites.net/.auth/login/twitter/callback`.
   >
   >

    Ha toouse korábban létrehozott hello titkok, adja hozzá a következő kód tooyour App delegált hello:

    **Objective-C**:

        #import <Fabric/Fabric.h>
        #import <TwitterKit/TwitterKit.h>
        // ...
        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
        {
            [[Twitter sharedInstance] startWithConsumerKey:@"your_key" consumerSecret:@"your_secret"];
            [Fabric with:@[[Twitter class]]];
            // Add any custom logic here.
            return YES;
        }

    **SWIFT**:

        import Fabric
        import TwitterKit
        // ...
        func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject : AnyObject]?) -> Bool {
            Twitter.sharedInstance().startWithConsumerKey("your_key", consumerSecret: "your_secret")
            Fabric.with([Twitter.self])
            // Add any custom logic here.
            return true
        }
3. Adja hozzá a következő kód tooyour alkalmazás, használ toohello nyelv szerint hello.

**Objective-C**:

    #import <TwitterKit/TwitterKit.h>
    // ...
    - (void)authenticate:(UIViewController*)parent completion:(void (^) (MSUser*, NSError*))completionBlock
    {
        [[Twitter sharedInstance] logInWithCompletion:^(TWTRSession *session, NSError *error) {
            if (session) {
                NSDictionary *payload = @{
                                            @"access_token":session.authToken,
                                            @"access_token_secret":session.authTokenSecret
                                        };
                [client loginWithProvider:@"twitter" token:payload completion:completionBlock];
            } else {
                completionBlock(nil, error);
            }
        }];
    }

**SWIFT**:

    import TwitterKit
    // ...
    func authenticate(parent: UIViewController, completion: (MSUser?, NSError?) -> Void) {
        let client = self.table!.client
        Twitter.sharedInstance().logInWithCompletion { session, error in
            if (session != nil) {
                let payload: [String: String] = ["access_token": session!.authToken, "access_token_secret": session!.authTokenSecret]
                client.loginWithProvider("twitter", token: payload, completion: completion)
            } else {
                completion(nil, error)
            }
        }
    }

## <a name="google-sdk"></a>Hogyan: hello Google bejelentkezési SDK iOS-hez a felhasználók hitelesítéséhez
Is használhatja hello Google bejelentkezési SDK iOS-toosign felhasználóknak az alkalmazás a Google-fiók használatával.  Google nemrég jelentette be módosítások tootheir OAuth biztonsági házirendeket.  A módosítások a házirend a Google SDK-t jövőbeli hello hello használata szükséges.

1. A mobil-háttéralkalmazás Google-bejelentkezés a következő hello konfigurálása [hogyan tooconfigure App Service Google bejelentkezési azonosítóhoz](app-service-mobile-how-to-configure-google-authentication.md) oktatóanyag.
2. Telepítse a Google SDK. hello következő hello által az iOS [Google bejelentkezhet az iOS - Start integrálása](https://developers.google.com/identity/sign-in/ios/start-integrating) dokumentációját. Előfordulhat, hogy átugorja hello "Hitelesítéséhez és a háttérkiszolgáló" szakaszt.
3. Adja hozzá a következő tooyour delegált hello `signIn:didSignInForUser:withError:` módszer szerint toohello nyelvet használ.

**Objective-C**:

        NSDictionary *payload = @{
                                  @"id_token":user.authentication.idToken,
                                  @"authorization_code":user.serverAuthCode
                                  };

        [client loginWithProvider:@"google" token:payload completion:^(MSUser *user, NSError *error) {
            // ...
        }];

**SWIFT**:

        let payload: [String: String] = ["id_token": user.authentication.idToken, "authorization_code": user.serverAuthCode]
        client.loginWithProvider("google", token: payload) { (user, error) in
            // ...
        }

1. Győződjön meg arról is hozzáadhat hello túl a következő`application:didFinishLaunchingWithOptions:` delegálása az alkalmazás, "SERVER_CLIENT_ID" lecserélését hello azonos azonosító, hogy Ön használt tooconfigure App Service az 1. lépésben.

**Objective-C**:

         [GIDSignIn sharedInstance].serverClientID = @"SERVER_CLIENT_ID";

 **SWIFT**:

        GIDSignIn.sharedInstance().serverClientID = "SERVER_CLIENT_ID"


1. Adja hozzá a következő kód tooyour alkalmazást egy UIViewController hello megvalósító hello `GIDSignInUIDelegate` protokoll szerinti toohello nyelvet használ.  Be van jelentkezve mielőtt újra éppen bejelentkezett, és nincs szüksége tooenter a hitelesítő adatok újra, bár látja-e a hozzájárulási párbeszédablak.  Ez a metódus csak hívható, ha hello munkameneti jogkivonat lejárt.

   **Objective-C**:

       #import <Google/SignIn.h>
       // ...
       - (void)authenticate
       {
               [GIDSignIn sharedInstance].uiDelegate = self;
               [[GIDSignIn sharedInstance] signOut];
               [[GIDSignIn sharedInstance] signIn];
        }

   **SWIFT**:

       // ...
       func authenticate() {
           GIDSignIn.sharedInstance().uiDelegate = self
           GIDSignIn.sharedInstance().signOut()
           GIDSignIn.sharedInstance().signIn()
       }

<!-- Anchors. -->

[What is Mobile Services]: #what-is
[Concepts]: #concepts
[Setup and Prerequisites]: #Setup
[How to: Create hello Mobile Services client]: #create-client
[How to: Create a table reference]: #table-reference
[How to: Query data from a mobile service]: #querying
[Filter returned data]: #filtering
[Sort returned data]: #sorting
[Return data in pages]: #paging
[Select specific columns]: #selecting
[How to: Bind data toohello user interface]: #binding
[How to: Insert data into a mobile service]: #inserting
[How to: Modify data in a mobile service]: #modifying
[How to: Authenticate users]: #authentication
[Cache authentication tokens]: #caching-tokens
[How to: Upload images and large files]: #blobs
[How to: Handle errors]: #errors
[How to: Design unit tests]: #unit-testing
[How to: Customize hello client]: #customizing
[Customize request headers]: #custom-headers
[Customize data type serialization]: #custom-serialization
[Next Steps]: #next-steps
[How to: Use MSQuery]: #query-object

<!-- Images. -->

<!-- URLs. -->
[Azure Mobile Apps gyors üzembe helyezés]: app-service-mobile-ios-get-started.md

[Add Mobile Services tooExisting App]: /develop/mobile/tutorials/get-started-data
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started-ios
[Validate and modify data in Mobile Services by using server scripts]: /develop/mobile/tutorials/validate-modify-and-augment-data-ios
[Mobile Services SDK]: https://go.microsoft.com/fwLink/p/?LinkID=266533
[Authentication]: /develop/mobile/tutorials/get-started-with-users-ios
[iOS SDK]: https://developer.apple.com/xcode

[Handling Expired Tokens]: http://go.microsoft.com/fwlink/p/?LinkId=301955
[Live Connect SDK]: http://go.microsoft.com/fwlink/p/?LinkId=301960
[Permissions]: http://msdn.microsoft.com/library/windowsazure/jj193161.aspx
[Service-side Authorization]: mobile-services-javascript-backend-service-side-authorization.md
[Use scripts tooauthorize users]: /develop/mobile/tutorials/authorize-users-in-scripts-ios
[dinamikus séma]: http://go.microsoft.com/fwlink/p/?LinkId=296271
[How to: access custom parameters]: /develop/mobile/how-to-guides/work-with-server-scripts#access-headers
[Create a table]: http://msdn.microsoft.com/library/windowsazure/jj193162.aspx
[NSDictionary object]: http://go.microsoft.com/fwlink/p/?LinkId=301965
[ASCII control codes C0 and C1]: http://en.wikipedia.org/wiki/Data_link_escape_character#C1_set
[CLI toomanage Mobile Services tables]: /cli/azure/get-started-with-az-cli2
[Conflict-Handler]: mobile-services-ios-handling-conflicts-offline-data.md#add-conflict-handling

[háló irányítópult]: https://www.fabric.io/home
[(iOS) – első lépések a háló]: https://docs.fabric.io/ios/fabric/getting-started.html
[1]: https://github.com/Azure/azure-mobile-apps-ios-client/blob/master/README.md#ios-client-sdk
[2]: http://azure.github.io/azure-mobile-apps-ios-client/
[3]: https://msdn.microsoft.com/library/azure/dn495101.aspx
[4]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#tags
[5]: http://azure.github.io/azure-mobile-services/iOS/v3/Classes/MSClient.html#//api/name/invokeAPI:data:HTTPMethod:parameters:headers:completion:
[6]: https://github.com/Azure/azure-mobile-services/blob/master/sdk/iOS/src/MSError.h
[7]: app-service-mobile-how-to-configure-active-directory-authentication.md
[8]: ../active-directory/active-directory-devquickstarts-ios.md
[9]: app-service-mobile-how-to-configure-facebook-authentication.md
[10]: https://developers.facebook.com/docs/ios/getting-started
