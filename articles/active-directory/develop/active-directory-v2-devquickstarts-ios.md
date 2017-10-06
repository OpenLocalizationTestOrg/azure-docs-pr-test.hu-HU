---
title: "az Azure AD v2.0-végponttól aaaAdd bejelentkezési tooan iOS alkalmazás használatával hello |} Microsoft Docs"
description: "Hogyan toobuild egy iOS-alkalmazást, amely jelentkezik be mindkét személyes Microsoft-fiókkal rendelkező felhasználók és a munkahelyi vagy iskolai fiókok külső könyvtárak használatával."
services: active-directory
documentationcenter: 
author: brandwe
manager: mbaldwin
editor: 
ms.assetid: fd3603c0-42f7-438c-87b5-a52d20d6344b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 01/07/2017
ms.author: brandwe
ms.custom: aaddev
ms.openlocfilehash: a384062e6e4bd398a2b12318800728e627e05c32
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-sign-in-tooan-ios-app-using-a-third-party-library-with-graph-api-using-hello-v20-endpoint"></a>Egy külső könyvtár használata a Graph API segítségével hello v2.0-végponttól bejelentkezési tooan iOS-alkalmazás hozzáadása
hello Microsoft identitásplatformmal például OAuth2 és az OpenID Connect nyitott szabványok használja. A fejlesztők a kívánják a szolgáltatások toointegrate függvénytárat. toohelp fejlesztők más könyvtárakkal a platformot használ, azt korábban írt néhány forgatókönyvek például a egy toodemonstrate hogyan tooconfigure külső szalagtárak tooconnect toohello Microsoft identitásplatformmal. A legtöbb tárak, amelyek megvalósítják az [hello RFC6749 OAuth2 spec](https://tools.ietf.org/html/rfc6749) toohello Microsoft identitásplatformmal kapcsolódhatnak.

Ez a forgatókönyv létrehozó hello alkalmazást, a felhasználók tootheir szervezet bejelentkezhet és majd keresse meg a többi a szervezetek hello Graph API segítségével.

Ha új tooOAuth2 vagy az OpenID Connect, ez a minta konfigurálási nem tehetik logika tooyou. Azt javasoljuk, hogy olvassa el [v2.0 protokoll - OAuth 2.0 hitelesítési kód Flow](active-directory-v2-protocols-oauth-code.md) a háttérben.

> [!NOTE]
> Egyes szolgáltatások, amelyek rendelkeznek egy kifejezés hello OAuth2 vagy OpenID Connect szabványok, például a feltételes hozzáférés és a Csoportházirend kezelése Intune-ban, a platform szükség akkor toouse a Microsoft Azure identitáskezelési szalagtárak nyílt forráskódú.
> 
> 

hello v2.0-végpontra nem támogatja az összes Azure Active Directory forgatókönyvek és funkciók.

> [!NOTE]
> toodetermine használatát hello v2.0-végpontra, olvassa el [v2.0 korlátozások](active-directory-v2-limitations.md).
> 
> 

## <a name="download-code-from-github"></a>Töltse le a kód a Githubról
az oktatóanyag kódjának hello kezelt [a Githubon](https://github.com/Azure-Samples/active-directory-ios-native-nxoauth2-v2).  toofollow mellett, akkor [töltse le a .zip hello alkalmazás vázát](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/skeleton.zip) vagy a Klónozás hello vázat:

```
git clone --branch skeleton git@github.com:Azure-Samples/active-directory-ios-native-nxoauth2-v2.git
```

Csak is letöltheti hello mintát, és rögtön használatba:

```
git clone git@github.com:Azure-Samples/active-directory-ios-native-nxoauth2-v2.git
```

## <a name="register-an-app"></a>Alkalmazás regisztrálása
Hozzon létre egy új alkalmazást hello [alkalmazásregisztrációs portálra](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), vagy hajtsa végre a részletes lépésekről hello [hogyan tooregister egy alkalmazást a v2.0-végponttól hello](active-directory-v2-app-registration.md).  Győződjön meg arról, hogy:

* Másolás hello **alkalmazásazonosító** , amely hozzárendelt tooyour app, mert hamarosan kell.
* Adja hozzá a hello **Mobile** platform az alkalmazásra vonatkozóan.
* Másolás hello **átirányítási URI-** hello portálról. Hello alapértelmezett értékét kell használnia `urn:ietf:wg:oauth:2.0:oob`.

## <a name="download-hello-third-party-nxoauth2-library-and-create-a-workspace"></a>Töltse le a hello külső NXOAuth2 könyvtárban, és egy munkaterület létrehozása
A forgatókönyv a Githubból, amely a Mac OS X és az iOS rendszerhez (Cocoa és Cocoa touch) OAuth2 könyvtár OAuth2Client hello fogja használni. Ezt a szalagtárat vázlat 10 a hello OAuth2 spec alapul. Hello natív alkalmazásprofil valósítja meg, és támogatja a hello engedélyezési végpont hello felhasználó. Az alábbiakban összes hello hello Microsoft identitásplatformmal együttműködve toointegrate lesz szüksége.

### <a name="add-hello-library-tooyour-project-by-using-cocoapods"></a>Hello könyvtár tooyour projekt hozzáadása a CocoaPods segítségével
A CocoaPods egy Xcode-projektekhez készült függőségkezelő. Hello korábbi telepítési lépéseket automatikusan kezeli.

```
$ vi Podfile
```
1. Adja hozzá a következő toothis podfile hello:
   
    ```
     platform :ios, '8.0'
   
     target 'QuickStart' do
   
     pod 'NXOAuth2Client'
   
     end
    ```
2. Hello podfile CocoaPods segítségével töltse be. Ezzel létrehoz egy új Xcode-munkaterületet.
   
    ```
    $ pod install
    ...
    $ open QuickStart.xcworkspace
    ```

## <a name="explore-hello-structure-of-hello-project"></a>Fedezze fel hello projekt hello szerkezete
a következő struktúra hello a projektre a hello vázat be van állítva:

* Egy nézet egy egyszerű keresés
* A részletes nézet hello adatok hello kiválasztott felhasználóról
* Ha egy felhasználó bejelentkezhessen a toohello app tooquery hello graph bejelentkezési nézet

Hello üres tooadd hitelesítési toovarious fájlok áthelyezi azt. Más részei hello kódok, például hello visual kód, nem vonatkoznak a tooidentity, de megadta-e meg.

## <a name="set-up-hello-settingsplst-file-in-hello-library"></a>Hello settings.plst fájl a könyvtárban hello beállítása
* Hello Gyorsútmutató-projekt, nyissa meg hello `settings.plist` fájlt. Cserélje le a hello szakasz tooreflect hello értékek hello Azure-portálon a használt hello elemeinek hello értékeit. A kód minden alkalommal hello Active Directory Authentication Library hivatkozik ezeket az értékeket.
  * Hello `clientId` hello ügyfél-azonosító az alkalmazás hello portálról másolt.
  * Hello `redirectUri` hello átirányítási URL-CÍMÉT, hogy a megadott hello portál van.

## <a name="set-up-hello-nxoauth2client-library-in-your-loginviewcontroller"></a>A LoginViewController NXOAuth2Client szalagtár hello beállítása
hello NXOAuth2Client tartalomtárát egyes értékek tooget beállítása. Miután elvégezte ezt a feladatot, hello szerzett be token toocall hello Graph API-t is használhatja. Mivel `LoginView` fogja meghívni bármikor tooauthenticate szükséges, az így tooput konfigurációs értékeket toothat fájlt.

* Adjunk néhány értékek toohello `LoginViewController.m` fájl tooset hello környezetben a hitelesítéshez és engedélyezéshez. Hello értékek részleteit hello kód kövesse.
  
    ```objc
    NSString *scopes = @"openid offline_access User.Read";
    NSString *authURL = @"https://login.microsoftonline.com/common/oauth2/v2.0/authorize";
    NSString *loginURL = @"https://login.microsoftonline.com/common/login";
    NSString *bhh = @"urn:ietf:wg:oauth:2.0:oob?code=";
    NSString *tokenURL = @"https://login.microsoftonline.com/common/oauth2/v2.0/token";
    NSString *keychain = @"com.microsoft.azureactivedirectory.samples.graph.QuickStart";
    static NSString * const kIDMOAuth2SuccessPagePrefix = @"session_state=";
    NSURL *myRequestedUrl;
    NSURL *myLoadedUrl;
    bool loginFlow = FALSE;
    bool isRequestBusy;
    NSURL *authcode;
    ```

Nézzük hello kód részleteit.

hello első karakterlánca a `scopes`.  Hello `User.Read` érték tooread hello alapvető profiladataihoz a bejelentkezett felhasználó hello lehetővé teszi.

Minden hello elérhető hatókörök kapcsolatos részletesebb [Microsoft Graph-engedélyhatókörök](https://graph.microsoft.io/docs/authorization/permission_scopes).

A `authURL`, `loginURL`, `bhh`, és `tokenURL`, korábban megadott hello értékek kell használnia. Ha hello nyílt forráskódú Microsoft Azure identitáskezelési-tárakat használ, azt lekérik a ezeket az adatokat, a metaadat-végpontjához használatával. A Microsoft régebben már kötöttek hello rögzített munkáját, ezek az értékek beolvasása.

Hello `keychain` hello tároló, amely NXOAuth2Client könyvtár hello fogja használni a kulcslánc-toostore toocreate a tokenek értéke. Ha szeretné tooget egyszeri bejelentkezés (SSO) számos alkalmazások között, megadhat hello azonos kulcslánc minden, az alkalmazások, és kérje, hogy az Xcode jogosultságok a kulcslánc hello használata. Ennek a magyarázatát a hello Apple dokumentációjában talál.

Ezeket az értékeket hello rest szükséges toouse hello könyvtár és helyen hozza létre toocarry értékek toohello környezetben.

### <a name="create-a-url-cache"></a>Egy URL-gyorsítótár létrehozása
Belül `(void)viewDidLoad()`, amely mindig neve után hello nézet be van töltve, hello alábbira primes jelek használata a gyorsítótárban.

Adja hozzá a következő kód hello:

```objc
- (void)viewDidLoad {
    [super viewDidLoad];
    self.loginView.delegate = self;
    [self setupOAuth2AccountStore];
    [self requestOAuth2Access];
    NSURLCache *URLCache = [[NSURLCache alloc] initWithMemoryCapacity:4 * 1024 * 1024
                                                         diskCapacity:20 * 1024 * 1024
                                                             diskPath:nil];
    [NSURLCache setSharedURLCache:URLCache];

}
```

### <a name="create-a-webview-for-sign-in"></a>A bejelentkezés a webes nézet létrehozása
A webes nézet hello felhasználónak további tényező van például SMS szöveges üzenet (Ha be van állítva), vagy hiba üzenetek toohello felhasználói adja vissza. Itt fogja beállított hello webes nézet és majd később írási hello kód toohandle hello visszahívások történjen hello webes nézet hello identitás szolgáltatásokból.

```objc
-(void)requestOAuth2Access {
    //toosign in tooMicrosoft APIs using OAuth2, we must show an embedded browser (UIWebView)
    [[NXOAuth2AccountStore sharedStore] requestAccessToAccountWithType:@"myGraphService"
                                   withPreparedAuthorizationURLHandler:^(NSURL *preparedURL) {
                                       //navigate toohello URL returned by NXOAuth2Client

                                       NSURLRequest *r = [NSURLRequest requestWithURL:preparedURL];
                                       [self.loginView loadRequest:r];
                                   }];
}
```

### <a name="override-hello-webview-methods-toohandle-authentication"></a>Hello webes nézet módszerek toohandle hitelesítési felülbírálása
tootell hello webes nézet, mi történik, ha a felhasználó számára szükséges toosign a korábban bemutatott, illessze be a következő kód hello.

```objc
- (void)resolveUsingUIWebView:(NSURL *)URL {

    // We get hello auth token from a redirect so we need toohandle that in hello webview.

    if (![NSThread isMainThread]) {
        [self performSelectorOnMainThread:@selector(resolveUsingUIWebView:) withObject:URL waitUntilDone:YES];
        return;
    }

    NSURLRequest *hostnameURLRequest = [NSURLRequest requestWithURL:URL cachePolicy:NSURLRequestUseProtocolCachePolicy timeoutInterval:10.0f];
    isRequestBusy = YES;
    [self.loginView loadRequest:hostnameURLRequest];

    NSLog(@"resolveUsingUIWebView ready (status: UNKNOWN, URL: %@)", self.loginView.request.URL);
}

- (BOOL)webView:(UIWebView *)webView shouldStartLoadWithRequest:(NSURLRequest *)request navigationType:(UIWebViewNavigationType)navigationType {

    NSLog(@"webView:shouldStartLoadWithRequest: %@ (%li)", request.URL, (long)navigationType);

    // hello webview is where all hello communication happens. Slightly complicated.

    myLoadedUrl = [webView.request mainDocumentURL];
    NSLog(@"***Loaded url: %@", myLoadedUrl);

    //if hello UIWebView is showing our authorization URL or consent URL, show hello UIWebView control
    if ([request.URL.absoluteString rangeOfString:authURL options:NSCaseInsensitiveSearch].location != NSNotFound) {
        self.loginView.hidden = NO;
    } else if ([request.URL.absoluteString rangeOfString:loginURL options:NSCaseInsensitiveSearch].location != NSNotFound) {
        //otherwise hide hello UIWebView, we've left hello authorization flow
        self.loginView.hidden = NO;
    } else if ([request.URL.absoluteString rangeOfString:bhh options:NSCaseInsensitiveSearch].location != NSNotFound) {
        //otherwise hide hello UIWebView, we've left hello authorization flow
        self.loginView.hidden = YES;
        [[NXOAuth2AccountStore sharedStore] handleRedirectURL:request.URL];
    }
    else {
        self.loginView.hidden = NO;
        //read hello Location from hello UIWebView, this is how Microsoft APIs is returning the
        //authentication code and relation information. This is controlled by hello redirect URL we chose toouse from Microsoft APIs
        //continue hello OAuth2 flow
       // [[NXOAuth2AccountStore sharedStore] handleRedirectURL:request.URL];
    }

    return YES;

}
```

### <a name="write-code-toohandle-hello-result-of-hello-oauth2-request"></a>Kód toohandle hello hello OAuth2 kérelem eredményét írása
hello alábbira kezelnek hello redirectURL, amely a webes nézet hello ad vissza. Ha a hitelesítés nem volt sikeres, hello kódot újra próbálkozik. Eközben hello könyvtár hello hiba hello konzolon látható elemek vagy aszinkron módon kezelésére ad meg.

```objc
- (void)handleOAuth2AccessResult:(NSString *)accessResult {

    AppData* data = [AppData getInstance];

    //parse hello response for success or failure
     if (accessResult)
    //if success, complete hello OAuth2 flow by handling hello redirect URL and obtaining a token
     {
         [[NXOAuth2AccountStore sharedStore] handleRedirectURL:accessResult];
    } else {
        //start over
        [self requestOAuth2Access];
    }
}
```

### <a name="set-up-hello-oauth-context-called-account-store"></a>Hello (néven fióktároló) OAuth környezet beállítása
Itt hívása `-[NXOAuth2AccountStore setClientID:secret:authorizationURL:tokenURL:redirectURL:forAccountType:]` a hello megosztott fióktároló az egyes szolgáltatásokhoz, amelyet az hello alkalmazás toobe képes tooaccess. hello fiók típusa: egy bizonyos szolgáltatás azonosítóként használt karakterlánc. Hello Graph API-val érik el, mert hello kód hivatkozik, tooit `"myGraphService"`. Majd állítsa be, amely jelzi, ha bármilyen változás hello jogkivonatok megfigyelő. Miután hello jogkivonat beszerzéséhez hello felhasználói hátsó toohello vissza `masterView`.

```objc
- (void)setupOAuth2AccountStore {


        AppData* data = [AppData getInstance];

    [[NXOAuth2AccountStore sharedStore] setClientID:data.clientId
                                             secret:data.secret
                                              scope:[NSSet setWithObject:scopes]
                                   authorizationURL:[NSURL URLWithString:authURL]
                                           tokenURL:[NSURL URLWithString:tokenURL]
                                        redirectURL:[NSURL URLWithString:data.redirectUriString]
                                      keyChainGroup: keychain
                                     forAccountType:@"myGraphService"];

    [[NSNotificationCenter defaultCenter] addObserverForName:NXOAuth2AccountStoreAccountsDidChangeNotification
                                                      object:[NXOAuth2AccountStore sharedStore]
                                                       queue:nil
                                                  usingBlock:^(NSNotification *aNotification) {
                                                      if (aNotification.userInfo) {
                                                          //account added, we have access
                                                          //we can now request protected data
                                                          NSLog(@"Success!! We have an access token.");
                                                          dispatch_async(dispatch_get_main_queue(),^ {

                                                              MasterViewController* masterViewController = [self.storyboard instantiateViewControllerWithIdentifier:@"masterView"];
                                                              [self.navigationController pushViewController:masterViewController animated:YES];
                                                          });
                                                      } else {
                                                          //account removed, we lost access
                                                      }
                                                  }];

    [[NSNotificationCenter defaultCenter] addObserverForName:NXOAuth2AccountStoreDidFailToRequestAccessNotification
                                                      object:[NXOAuth2AccountStore sharedStore]
                                                       queue:nil
                                                  usingBlock:^(NSNotification *aNotification) {
                                                      NSError *error = [aNotification.userInfo objectForKey:NXOAuth2AccountStoreErrorKey];
                                                      NSLog(@"Error!! %@", error.localizedDescription);
                                                  }];
}
```

## <a name="set-up-hello-master-view-toosearch-and-display-hello-users-from-hello-graph-api"></a>Nézet toosearch hello beállítása és a Graph API hello hello felhasználók megjelenítése
Egy Master-vezérlő (MVC) alkalmazást, amely megjeleníti a visszaadott adatok hello rácsban hello nem ez a forgatókönyv hello terjed, és sok online oktatóanyagok azt ismertetik, hogyan toobuild egyet. Ez a kód hello üres fájl van. Az MVC alkalmazás néhány dolgot a toodeal szüksége:

* Ha egy felhasználó egy keresőmezőt hello INTERCEPT
* Adatok hátsó toohello MasterView objektum biztosítanak, így hello eredmények hello rácsban megjeleníti

Igazolnia kell végeznie ezeket az alábbi.

### <a name="add-a-check-toosee-if-youre-logged-in"></a>Adja hozzá a jelölőnégyzet toosee, ha jelentkezett be
hello alkalmazás hello felhasználó nem jelentkezett be, ha kevés teszi, hogy az intelligens toocheck Ha már van egy token hello gyorsítótárában. Ha nem, a hello felhasználói toosign LoginView toohello irányítja át a. Emlékezzen vissza, ha hello legjobb módja toodo műveletek nézet betöltésekor-e toouse hello `viewDidLoad()` módszer, amely Apple velünk.

```objc
- (void)viewDidLoad {
    [super viewDidLoad];


    NXOAuth2AccountStore *store = [NXOAuth2AccountStore sharedStore];
    NSArray *accounts = [store accountsWithAccountType:@"myGraphService"];

        if (accounts.count == 0) {

        dispatch_async(dispatch_get_main_queue(),^ {

            LoginViewController* userSelectController = [self.storyboard instantiateViewControllerWithIdentifier:@"LoginUserView"];
            [self.navigationController pushViewController:userSelectController animated:YES];
        });
        }
```

### <a name="update-hello-table-view-when-data-is-received"></a>Frissítés hello tábla nézet adatainak fogadásakor.
A Graph API hello adatait jeleníti meg, úgy kell toodisplay hello adatokat. Az egyszerűség kedvéért itt található összes hello kód tooupdate hello tábla. Csak az MVC bolierplate kódjában illessze be hello megfelelő értékeket.

```objc
#pragma mark - Table View

- (NSInteger)numberOfSectionsInTableView:(UITableView *)tableView {
    return 1;
}

- (NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section {

        return [upnArray count];
}

- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath {

    UITableViewCell *cell = [tableView dequeueReusableCellWithIdentifier:@"TaskPrototypeCell" forIndexPath:indexPath];

    if ( cell == nil ) {
        cell = [[UITableViewCell alloc] initWithStyle:UITableViewCellStyleDefault reuseIdentifier:@"TaskPrototypeCell"];
    }

    User *user = nil;
     user = [upnArray objectAtIndex:indexPath.row];


    // Configure hello cell
    cell.textLabel.text = user.name;
    [cell setAccessoryType:UITableViewCellAccessoryDisclosureIndicator];

    return cell;
}

```

### <a name="provide-a-way-toocall-hello-graph-api-when-someone-types-in-hello-search-field"></a>Adjon meg egy módon toocall hello Graph API-val, ha valaki hello keresési mező
Ha egy felhasználó egy keresési hello mezőben, tooshove kell, a Graph API toohello keresztül. Hello `GraphAPICaller` osztályt, amely a következő kód hello fog létrehozni, hello bemutató elválasztja hello keresési funkciót. Most ideje lefuttatni bármely keresési karakterek toohello Graph API-hírcsatornák hello kódját. A Microsoft ehhez a hívott metódus megadásával `lookupInGraph`, így tovább is, amely azt szeretnénk, ha a toosearch hello karakterlánc.

```objc

-(void)lookupInGraph:(NSString *)searchText {
if (searchText.length > 0) {

    };



        [GraphAPICaller searchUserList:searchText completionBlock:^(NSMutableArray* returnedUpns, NSError* error) {
            if (returnedUpns) {


                upnArray = returnedUpns;


            }
            else
            {
                UIAlertView *alertView = [[UIAlertView alloc]initWithTitle:nil message:[[NSString alloc]initWithFormat:@"Error : %@", error.localizedDescription] delegate:nil cancelButtonTitle:@"Retry" otherButtonTitles:@"Cancel", nil];

                [alertView setDelegate:self];

                dispatch_async(dispatch_get_main_queue(),^ {
                    [alertView show];
                });
            }


        }];


}
```

## <a name="write-a-helper-class-tooaccess-hello-graph-api"></a>Írás a segítő osztály tooaccess hello Graph API-val
Ez a hello mag az alkalmazás. Mivel hello rest lett beszúrva kód hello alapértelmezett MVC mintában az Apple-től, itt írt kódot tooquery hello graph hello felhasználói meg kell adnia, és térjen vissza az adatokat. Hello kódot ide, és részletesen ismerteti az azt követő.

### <a name="create-a-new-objective-c-header-file"></a>Hozzon létre egy új Objective C-fejléc fájlt
Nevű hello fájl `GraphAPICaller.h`, és adja hozzá a következő kód hello.

```objc
@interface GraphAPICaller : NSObject<NSURLConnectionDataDelegate>

+(void) searchUserList:(NSString*)searchString
       completionBlock:(void (^) (NSMutableArray*, NSError* error))completionBlock;

@end
```

Itt láthatja, hogy a megadott metódus lép egy karakterláncot, és egy completionBlock adja vissza. A completionBlock, akkor előfordulhat, hogy rendelkezik kitalál, frissíteni fogja hello tábla azáltal objektum feltöltött adatok valós idejű, a felhasználó keresések hello.

### <a name="create-a-new-objective-c-file"></a>Hozzon létre egy új Objective C-fájlt
Nevű hello fájl `GraphAPICaller.m`, és adja hozzá a következő metódus hello.

```objc
+(void) searchUserList:(NSString*)searchString
       completionBlock:(void (^) (NSMutableArray* Users, NSError* error)) completionBlock
{
    if (!loadedApplicationSettings)
    {
        [self readApplicationSettings];
    }

    AppData* data = [AppData getInstance];

    NSString *graphURL = [NSString stringWithFormat:@"%@%@/users", data.graphApiUrlString, data.apiversion];

    NXOAuth2AccountStore *store = [NXOAuth2AccountStore sharedStore];
    NSDictionary* params = [self convertParamsToDictionary:searchString];

    NSArray *accounts = [store accountsWithAccountType:@"myGraphService"];
    [NXOAuth2Request performMethod:@"GET"
                        onResource:[NSURL URLWithString:graphURL]
                   usingParameters:params
                       withAccount:accounts[0]
               sendProgressHandler:^(unsigned long long bytesSend, unsigned long long bytesTotal) {
                   // e.g., update a progress indicator
               }
                   responseHandler:^(NSURLResponse *response, NSData *responseData, NSError *error) {
                       // Process hello response
                       if (responseData) {
                           NSError *error;
                           NSDictionary *dataReturned = [NSJSONSerialization JSONObjectWithData:responseData options:0 error:nil];
                           NSLog(@"Graph Response was: %@", dataReturned);

                           // We can grab hello top most JSON node tooget our graph data.
                           NSArray *graphDataArray = [dataReturned objectForKey:@"value"];

                           // Don't be thrown off by hello key name being "value". It really is hello name of the
                           // first node. :-)

                           //each object is a key value pair
                           NSDictionary *keyValuePairs;
                           NSMutableArray* Users = [[NSMutableArray alloc]init];

                           for(int i =0; i < graphDataArray.count; i++)
                           {
                               keyValuePairs = [graphDataArray objectAtIndex:i];

                               User *s = [[User alloc]init];
                               s.upn = [keyValuePairs valueForKey:@"userPrincipalName"];
                               s.name =[keyValuePairs valueForKey:@"displayName"];
                               s.mail =[keyValuePairs valueForKey:@"mail"];
                               s.businessPhones =[keyValuePairs valueForKey:@"businessPhones"];
                               s.mobilePhones =[keyValuePairs valueForKey:@"mobilePhone"];


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

```

Ezzel a módszerrel részletesen lépjen.

Ez a kód hello mag van hello `NXOAuth2Request`, metódust hello paramétereket, amelyek már megadott hello settings.plist fájlban.

hello első lépéseként tooconstruct hello jobb Graph API-hívás, amely. Mivel a hívott `/users`, megadja, hogy toohello Graph API erőforrás hello verziójával együtt hozzáfűzésével. Teszi logika tooput egy külső beállítások fájlba, mert ezek hello API fejlődésének módosíthatja.

```objc
NSString *graphURL = [NSString stringWithFormat:@"%@%@/users", data.graphApiUrlString, data.apiversion];
```

A következő lépésben toospecify paraméterek toohello Graph API-hívások is megadja. Az *nagyon fontos* , hogy nem helyezett hello paraméterek hello erőforrás végpont mert, amely az összes nem-URI megfelelő karaktereket futásidőben van törlődik. Az összes lekérdezés kód hello paramétereket kell megadni.

```objc

NSDictionary* params = [self convertParamsToDictionary:searchString];
```

Bizonyára észrevette, hogy meghívja a `convertParamsToDictionary` módszer, amely még nem írt. Tekintsük át most hello fájl hello végén:

```objc
+(NSDictionary*) convertParamsToDictionary:(NSString*)searchString
{
    NSMutableDictionary* dictionary = [[NSMutableDictionary alloc]init];

        NSString *query = [NSString stringWithFormat:@"startswith(givenName, '%@')", searchString];

           [dictionary setValue:query forKey:@"$filter"];



    return dictionary;
}

```
A következő most használja az hello `NXOAuth2Request` metódus tooget adatok biztonsági hello API JSON formátumban.

```objc
NSArray *accounts = [store accountsWithAccountType:@"myGraphService"];
    [NXOAuth2Request performMethod:@"GET"
                        onResource:[NSURL URLWithString:graphURL]
                   usingParameters:params
                       withAccount:accounts[0]
               sendProgressHandler:^(unsigned long long bytesSend, unsigned long long bytesTotal) {
                   // e.g., update a progress indicator
               }
                   responseHandler:^(NSURLResponse *response, NSData *responseData, NSError *error) {
                       // Process hello response
                       if (responseData) {
                           NSError *error;
                           NSDictionary *dataReturned = [NSJSONSerialization JSONObjectWithData:responseData options:0 error:nil];
                           NSLog(@"Graph Response was: %@", dataReturned);

                           // We can grab hello top most JSON node tooget our graph data.
                           NSArray *graphDataArray = [dataReturned objectForKey:@"value"];
```

Végezetül nézzük hogyan hello adatok toohello MasterViewController adja vissza. hello adatokat adja vissza, mert a szerializált kell deszerializálni toobe és töltve az objektum az adott hello MainViewController felhasználhat. Erre a célra hello vázat tartalmaz egy `User.m/h` fájlt, amely a felhasználó-objektumot hoz létre. Feltölti felhasználói objektum hello graph származó információkkal.

```objc
                           // We can grab hello top most JSON node tooget our graph data.
                           NSArray *graphDataArray = [dataReturned objectForKey:@"value"];

                           // Don't be thrown off by hello key name being "value". It really is hello name of the
                           // first node. :-)

                           //each object is a key value pair
                           NSDictionary *keyValuePairs;
                           NSMutableArray* Users = [[NSMutableArray alloc]init];

                           for(int i =0; i < graphDataArray.count; i++)
                           {
                               keyValuePairs = [graphDataArray objectAtIndex:i];

                               User *s = [[User alloc]init];
                               s.upn = [keyValuePairs valueForKey:@"userPrincipalName"];
                               s.name =[keyValuePairs valueForKey:@"displayName"];
                               s.mail =[keyValuePairs valueForKey:@"mail"];
                               s.businessPhones =[keyValuePairs valueForKey:@"businessPhones"];
                               s.mobilePhones =[keyValuePairs valueForKey:@"mobilePhone"];


                               [Users addObject:s];
```


## <a name="run-hello-sample"></a>Hello minta futtatásához
Ha hello vázat használt vagy együtt hello forgatókönyv követni az alkalmazás most már működik. Hello szimulátorban történő elindításához, és kattintson a **bejelentkezés** toouse hello alkalmazás.

## <a name="get-security-updates-for-our-product"></a>A termék biztonsági frissítések beszerzése
Javasoljuk, tooget értesítést a bekövetkező biztonsági incidensekről hello felkeresésével [biztonsági TechCenter](https://technet.microsoft.com/security/dd252948) és előfizetés tooSecurity tanácsadói riasztásokra.

