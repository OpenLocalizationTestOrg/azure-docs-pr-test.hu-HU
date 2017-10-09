---
title: "aaaAzure Notification Hubs – felhasználók értesítése iOS rendszerhez .NET-háttérrendszerrel"
description: "Ismerje meg, toosend a leküldéses értesítések toousers az Azure-ban. Kódminták Objective-C és hello .NET API hello háttérkiszolgáló."
documentationcenter: ios
author: ysxu
manager: erikre
editor: 
services: notification-hubs
ms.assetid: 1f7d1410-ef93-4c4b-813b-f075eed20082
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: ios
ms.devlang: objective-c
ms.topic: article
ms.date: 10/03/2016
ms.author: yuaxu
ms.openlocfilehash: 56aed5b04d2d985b3f0e50c58991607f07b61248
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-notification-hubs-notify-users-for-ios-with-net-backend"></a>Azure Notification Hubs – felhasználók értesítése iOS rendszerhez .NET-háttérrendszerrel
[!INCLUDE [notification-hubs-selector-aspnet-backend-notify-users](../../includes/notification-hubs-selector-aspnet-backend-notify-users.md)]

## <a name="overview"></a>Áttekintés
Leküldéses értesítési támogatás az Azure-ban lehetővé teszi tooaccess egy könnyen kezelhető, multiplatform és kibővített leküldéses infrastruktúrában, ami jelentősen egyszerűbb a leküldéses értesítések a mobile fogyasztói, valamint a vállalati alkalmazások hello végrehajtása platformok. Ez az oktatóanyag toouse Azure Notification Hubs toosend a leküldéses értesítések tooa adott alkalmazás felhasználói az adott eszközön. ASP.NET WebAPI háttérrendszerből használt tooauthenticate ügyfelek és toogenerate értesítéseket, ahogy hello útmutatást a témakör az [az alkalmazás háttérrendszeréből regisztrálása](notification-hubs-push-notification-registration-management.md#registration-management-from-a-backend).

> [!NOTE]
> Ez az oktatóanyag feltételezi, hogy létrehozta és leírtak szerint konfigurálta az értesítési központ [Ismerkedés a Notification Hubs (iOS)](notification-hubs-ios-apple-push-notification-apns-get-started.md). Ez az oktatóanyag egyben hello előfeltétel toohello [(iOS) biztonságos leküldéses](notification-hubs-aspnet-backend-ios-push-apple-apns-secure-notification.md) oktatóanyag.
> Ha a háttérkiszolgáló szolgáltatásként toouse Mobile Apps, lásd: hello [Mobile Apps első lépések leküldéses](../app-service-mobile/app-service-mobile-ios-get-started-push.md).
> 
> 

[!INCLUDE [notification-hubs-aspnet-backend-notifyusers](../../includes/notification-hubs-aspnet-backend-notifyusers.md)]

## <a name="modify-your-ios-app"></a>Az iOS-alkalmazás módosítása
1. Nyissa meg hello egylapos alkalmazás megtekintése hello létrehozott [Ismerkedés a Notification Hubs (iOS)](notification-hubs-ios-apple-push-notification-apns-get-started.md) oktatóanyag.
   
   > [!NOTE]
   > Ebben a szakaszban feltételezzük, hogy a projekt egy üres szervezetnév van konfigurálva. Ha nem, akkor tooprepend a szervezet neve tooall osztály nevét.
   > 
   > 
2. Adja hozzá a Main.storyboard hello összetevők hello objektum könyvtárból alatt hello képernyőfelvételen látható módon.
   
    ![][1]
   
   * **Felhasználónév**: a helyőrző szöveg A UITextField *adjon meg felhasználónevet*, azonnal hello alatt eredmények címke és a bal oldali korlátozott toohello küldését és jobb margók és hello alatt küldése az eredmények címke.
   * **Jelszó**: a helyőrző szöveg A UITextField *jelszó megadása*, azonnal kijelölése hello felhasználónév szövegmezőt és korlátozott toohello bal és jobb margók és hello felhasználónév szövegmező alatt. Ellenőrizze a hello **szövegbeviteli biztonságos** hello attribútum megtekintő lehetőséget a *vissza kulcs*.
   * **Jelentkezzen be**: A UIButton feliratú azonnal hello jelszó szövegmező alatt, és törölje a hello **engedélyezve** hello attribútumok megtekintő lehetőséget a *vezérlő – tartalom*
   * **WNS**: címke és kapcsoló tooenable küldése hello értesítési Windows értesítési szolgáltatásával, ha a telepítő hello központ le van. Lásd: hello [Windows bevezetés](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) oktatóanyag.
   * **GCM**: címke és kapcsoló tooenable küldése hello értesítési tooGoogle Cloud Messaging, ha a telepítő hello központ le van. Lásd: [Android bevezetés](notification-hubs-android-push-notification-google-gcm-get-started.md) oktatóanyag.
   * **APNS**: címke és kapcsoló tooenable küldése hello Apple Platform Notification szolgáltatáshoz értesítést toohello.
   * **Recipent felhasználónév**: a helyőrző szöveg A UITextField *címzett felhasználónév címke*, azonnal alatt hello GCM címke és korlátozott toohello bal és jobb margók és hello GCM alatt címkézését.

    Néhány összetevőt hozzáadott hello [Ismerkedés a Notification Hubs (iOS)](notification-hubs-ios-apple-push-notification-apns-get-started.md) oktatóanyag.

1. **CTRL** az egérrel húzzon át hello nézet tooViewController.h hello összetevőket, és adja hozzá ezek új értékesítési lehetőségek.
   
        @property (weak, nonatomic) IBOutlet UITextField *UsernameField;
        @property (weak, nonatomic) IBOutlet UITextField *PasswordField;
        @property (weak, nonatomic) IBOutlet UITextField *RecipientField;
        @property (weak, nonatomic) IBOutlet UITextField *NotificationField;
   
        // Used tooenable hello buttons on hello UI
        @property (weak, nonatomic) IBOutlet UIButton *LogInButton;
        @property (weak, nonatomic) IBOutlet UIButton *SendNotificationButton;
   
        // Used tooenabled sending notifications across platforms
        @property (weak, nonatomic) IBOutlet UISwitch *WNSSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *GCMSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *APNSSwitch;
   
        - (IBAction)LogInAction:(id)sender;
2. A ViewController.h, adja hozzá a hello következő `#define` alatt az importálási utasítást. Helyettesítő hello *< írja be a háttérkiszolgáló végpont\>*  helyőrzőt hello használt toodeploy a háttéralkalmazás hello előző szakaszban cél URL-címe. Például *http://you_backend.azurewebsites.net*.
   
        #define BACKEND_ENDPOINT @"<Enter Your Backend Endpoint>"
3. A projekt, hozzon létre egy új **Cocoa Touch osztály** nevű **RegisterClient** az ASP.NET-háttér létrehozott hello toointerface. Hozzon létre hello osztályt való öröklődés `NSObject`. Majd adja hozzá a következő kódot a hello RegisterClient.h hello.
   
        @interface RegisterClient : NSObject
   
        @property (strong, nonatomic) NSString* authenticationHeader;
   
        -(void) registerWithDeviceToken:(NSData*)token tags:(NSSet*)tags
            andCompletion:(void(^)(NSError*))completion;
   
        -(instancetype) initWithEndpoint:(NSString*)Endpoint;
   
        @end
4. Hello RegisterClient.m frissítse hello `@interface` szakasz:
   
        @interface RegisterClient ()
   
        @property (strong, nonatomic) NSURLSession* session;
        @property (strong, nonatomic) NSURLSession* endpoint;
   
        -(void) tryToRegisterWithDeviceToken:(NSData*)token tags:(NSSet*)tags retry:(BOOL)retry
                    andCompletion:(void(^)(NSError*))completion;
        -(void) retrieveOrRequestRegistrationIdWithDeviceToken:(NSString*)token
                    completion:(void(^)(NSString*, NSError*))completion;
        -(void) upsertRegistrationWithRegistrationId:(NSString*)registrationId deviceToken:(NSString*)token
                    tags:(NSSet*)tags andCompletion:(void(^)(NSURLResponse*, NSError*))completion;
   
        @end
5. Cserélje le a hello `@implementation` hello RegisterClient.m a következő kód hello szakaszában.

        @implementation RegisterClient

        // Globals used by RegisterClient
        NSString *const RegistrationIdLocalStorageKey = @"RegistrationId";

        -(instancetype) initWithEndpoint:(NSString*)Endpoint
        {
            self = [super init];
            if (self) {
                NSURLSessionConfiguration* config = [NSURLSessionConfiguration defaultSessionConfiguration];
                _session = [NSURLSession sessionWithConfiguration:config delegate:nil delegateQueue:nil];
                _endpoint = Endpoint;
            }
            return self;
        }

        -(void) registerWithDeviceToken:(NSData*)token tags:(NSSet*)tags
                    andCompletion:(void(^)(NSError*))completion
        {
            [self tryToRegisterWithDeviceToken:token tags:tags retry:YES andCompletion:completion];
        }

        -(void) tryToRegisterWithDeviceToken:(NSData*)token tags:(NSSet*)tags retry:(BOOL)retry
                    andCompletion:(void(^)(NSError*))completion
        {
            NSSet* tagsSet = tags?tags:[[NSSet alloc] init];

            NSString *deviceTokenString = [[token description]
                stringByTrimmingCharactersInSet:[NSCharacterSet characterSetWithCharactersInString:@"<>"]];
            deviceTokenString = [[deviceTokenString stringByReplacingOccurrencesOfString:@" " withString:@""]
                                    uppercaseString];

            [self retrieveOrRequestRegistrationIdWithDeviceToken: deviceTokenString
                completion:^(NSString* registrationId, NSError *error) {
                NSLog(@"regId: %@", registrationId);
                if (error) {
                    completion(error);
                    return;
                }

                [self upsertRegistrationWithRegistrationId:registrationId deviceToken:deviceTokenString
                    tags:tagsSet andCompletion:^(NSURLResponse * response, NSError *error) {
                    if (error) {
                        completion(error);
                        return;
                    }

                    NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*)response;
                    if (httpResponse.statusCode == 200) {
                        completion(nil);
                    } else if (httpResponse.statusCode == 410 && retry) {
                        [self tryToRegisterWithDeviceToken:token tags:tags retry:NO andCompletion:completion];
                    } else {
                        NSLog(@"Registration error with response status: %ld", (long)httpResponse.statusCode);

                        completion([NSError errorWithDomain:@"Registration" code:httpResponse.statusCode
                                    userInfo:nil]);
                    }

                }];
            }];
        }

        -(void) upsertRegistrationWithRegistrationId:(NSString*)registrationId deviceToken:(NSData*)token
                    tags:(NSSet*)tags andCompletion:(void(^)(NSURLResponse*, NSError*))completion
        {
            NSDictionary* deviceRegistration = @{@"Platform" : @"apns", @"Handle": token,
                                                    @"Tags": [tags allObjects]};
            NSData* jsonData = [NSJSONSerialization dataWithJSONObject:deviceRegistration
                                options:NSJSONWritingPrettyPrinted error:nil];

            NSLog(@"JSON registration: %@", [[NSString alloc] initWithData:jsonData
                                                encoding:NSUTF8StringEncoding]);

            NSString* endpoint = [NSString stringWithFormat:@"%@/api/register/%@", _endpoint,
                                    registrationId];
            NSURL* requestURL = [NSURL URLWithString:endpoint];
            NSMutableURLRequest* request = [NSMutableURLRequest requestWithURL:requestURL];
            [request setHTTPMethod:@"PUT"];
            [request setHTTPBody:jsonData];
            NSString* authorizationHeaderValue = [NSString stringWithFormat:@"Basic %@",
                                                    self.authenticationHeader];
            [request setValue:authorizationHeaderValue forHTTPHeaderField:@"Authorization"];
            [request setValue:@"application/json" forHTTPHeaderField:@"Content-Type"];

            NSURLSessionDataTask* dataTask = [self.session dataTaskWithRequest:request
                completionHandler:^(NSData *data, NSURLResponse *response, NSError *error)
            {
                if (!error)
                {
                    completion(response, error);
                }
                else
                {
                    NSLog(@"Error request: %@", error);
                    completion(nil, error);
                }
            }];
            [dataTask resume];
        }

        -(void) retrieveOrRequestRegistrationIdWithDeviceToken:(NSString*)token
                    completion:(void(^)(NSString*, NSError*))completion
        {
            NSString* registrationId = [[NSUserDefaults standardUserDefaults]
                                        objectForKey:RegistrationIdLocalStorageKey];

            if (registrationId)
            {
                completion(registrationId, nil);
                return;
            }

            // request new one & save
            NSURL* requestURL = [NSURL URLWithString:[NSString stringWithFormat:@"%@/api/register?handle=%@",
                                    _endpoint, token]];
            NSMutableURLRequest* request = [NSMutableURLRequest requestWithURL:requestURL];
            [request setHTTPMethod:@"POST"];
            NSString* authorizationHeaderValue = [NSString stringWithFormat:@"Basic %@",
                                                    self.authenticationHeader];
            [request setValue:authorizationHeaderValue forHTTPHeaderField:@"Authorization"];

            NSURLSessionDataTask* dataTask = [self.session dataTaskWithRequest:request
                completionHandler:^(NSData *data, NSURLResponse *response, NSError *error)
            {
                NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
                if (!error && httpResponse.statusCode == 200)
                {
                    NSString* registrationId = [[NSString alloc] initWithData:data
                        encoding:NSUTF8StringEncoding];

                    // remove quotes
                    registrationId = [registrationId substringWithRange:NSMakeRange(1,
                                        [registrationId length]-2)];

                    [[NSUserDefaults standardUserDefaults] setObject:registrationId
                        forKey:RegistrationIdLocalStorageKey];
                    [[NSUserDefaults standardUserDefaults] synchronize];

                    completion(registrationId, nil);
                }
                else
                {
                    NSLog(@"Error status: %ld, request: %@", (long)httpResponse.statusCode, error);
                    if (error)
                        completion(nil, error);
                    else {
                        completion(nil, [NSError errorWithDomain:@"Registration" code:httpResponse.statusCode
                                    userInfo:nil]);
                    }
                }
            }];
            [dataTask resume];
        }

        @end

    a fenti hello kódot megvalósítja hello útmutatást a cikkben ismertetett hello logika [az alkalmazás háttérrendszeréből regisztrálása](notification-hubs-push-notification-registration-management.md#registration-management-from-a-backend) NSURLSession használatával tooperform REST meghívja tooyour háttéralkalmazás és NSUserDefaults toolocally tároló hello hello értesítési központ által visszaadott registrationId.

    Vegye figyelembe, hogy ez az osztály van szükség a tulajdonsága **authorizationHeader** toobe rendelés toowork megfelelően beállítva. A tulajdonság értéke által hello **ViewController** osztály után hello jelentkezzen be.

1. A ViewController.h, adjon hozzá egy `#import` RegisterClient.h utasítást. Majd adjon hozzá egy; hello eszköz jogkivonat, és hivatkozzon tooa `RegisterClient` példánya a hello `@interface` szakasz:
   
        #import "RegisterClient.h"
   
        @property (strong, nonatomic) NSData* deviceToken;
        @property (strong, nonatomic) RegisterClient* registerClient;
2. ViewController.m, adja hozzá a titkos metódus deklarációjában a hello `@interface` szakasz:
   
        @interface ViewController () <UITextFieldDelegate, NSURLConnectionDataDelegate, NSXMLParserDelegate>
   
        // create hello Authorization header tooperform Basic authentication with your app back-end
        -(void) createAndSetAuthenticationHeaderWithUsername:(NSString*)username
                        AndPassword:(NSString*)password;
   
        @end

> [!NOTE]
> hello alábbi részlet nem egy biztonságos hitelesítési séma, helyettesítse be hello hello végrehajtásának **createAndSetAuthenticationHeaderWithUsername:AndPassword:** , az adott hitelesítési mechanizmus egy hello register ügyfél osztály, például az OAuth, az Active Directory által használt hitelesítési jogkivonat toobe létrehozó.
> 
> 

1. Ezt a hello `@implementation` ViewController.m szakasza adja hozzá a következő kód, amely hello megvalósítási beállítás hello eszköz token és hitelesítési fejléc hozzáadása hello.
   
        -(void) setDeviceToken: (NSData*) deviceToken
        {
            _deviceToken = deviceToken;
            self.LogInButton.enabled = YES;
        }
   
        -(void) createAndSetAuthenticationHeaderWithUsername:(NSString*)username
                        AndPassword:(NSString*)password;
        {
            NSString* headerValue = [NSString stringWithFormat:@"%@:%@", username, password];
   
            NSData* encodedData = [[headerValue dataUsingEncoding:NSUTF8StringEncoding] base64EncodedDataWithOptions:NSDataBase64EncodingEndLineWithCarriageReturn];
   
            self.registerClient.authenticationHeader = [[NSString alloc] initWithData:encodedData
                                                        encoding:NSUTF8StringEncoding];
        }
   
        -(BOOL)textFieldShouldReturn:(UITextField *)textField
        {
            [textField resignFirstResponder];
            return YES;
        }
   
    Vegye figyelembe, hogyan beállítás hello eszköz jogkivonatát lehetővé teszi, hogy hello bejelentkezés gombra. Ennek az az oka hello bejelentkezési művelet részeként a hello nézetvezérlő regisztrálja a leküldéses értesítések az hello háttéralkalmazás. Emiatt nem szeretnénk bejelentkezési művelet toobe elérhető keretein belül hello eszköz jogkivonatát megfelelően be van állítva. Hello leküldéses regisztráció a bejelentkezéshez hello is absztrakcióhoz, mindaddig, amíg hello volt ez utóbbi hello előtt történik.
2. A ViewController.m, használja az alábbi kódtöredékek tooimplement hello tartozó műveleti módszer a hello a **bejelentkezés** gombra, és egy metódus toosend hello értesítési üzenet használatával hello ASP.NET-háttérrendszerből.
   
       - (IBAction) LogInAction: (id) küldő {/ / hitelesítési fejléc létrehozásáról és a nyilvántartás ügyfél NSString * felhasználónév = saját maga. UsernameField.text;   NSString * jelszó = saját maga. PasswordField.text;
   
           [az önkiszolgáló createAndSetAuthenticationHeaderWithUsername:username AndPassword:password];
   
           __weak ViewController * selfie = önkiszolgáló;   [self.registerClient registerWithDeviceToken:self.deviceToken címkék: üres andCompletion:^(NSError* error) {Ha (! hiba) {dispatch_async(dispatch_get_main_queue(), ^ {selfie. SendNotificationButton.enabled = YES;               [self MessageBox:@"Success" message:@"Registered sikeresen!"];});}}];}

        -SendNotificationASPNETBackend (a "void"): (NSString*) pns UsernameTag: (NSString*) usernameTag üzenetet: (NSString*) üzenet {NSURLSession* munkamenet = [NSURLSession sessionWithConfiguration: [ NSURLSessionConfiguration defaultSessionConfiguration] delegált: üres delegateQueue:nil];

            A hello többi URL-cím toohello ASP.NET háttér által igényelt NSURL * requestURL paraméterként hello pns-sel és a username címke átadni = [által igényelt NSURL URLWithString: [NSString stringWithFormat:@"%@/api/notifications? pns = % @& to_tag = % @", BACKEND_ENDPOINT, pns, usernameTag]];

            NSMutableURLRequest * kérelem = [NSMutableURLRequest requestWithURL:requestURL];    [lekérő setHTTPMethod:@"POST"];

            Hello utánzatait authenticationheader lehívása hello register ügyfél NSString * authorizationHeaderValue = [NSString stringWithFormat:@"Basic % @", self.registerClient.authenticationHeader];    [lekérő setValue:authorizationHeaderValue forHTTPHeaderField:@"Authorization"];

            Adja hozzá a hello értesítési üzenet törzse [setValue:@"application/json;charset=utf-8 kérelem" forHTTPHeaderField:@"Content-Type"];    [setHTTPBody kérése: [üzenet dataUsingEncoding:NSUTF8StringEncoding]];

            Hello ASP.NET háttér NSURLSessionDataTask * dataTask hello send notification REST API-t végrehajtása [munkamenet dataTaskWithRequest:request completionHandler:^(NSData *data, NSURLResponse *response, NSError  *= Hiba történt) {NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) választ;        Ha (hiba || httpResponse.statusCode! = 200) {NSString* állapot = [NSString stringWithFormat:@"Error állapota a(z) % @: % d\nError: %@\n", pns, httpResponse.statusCode, hiba];            dispatch_async(dispatch_get_main_queue(), ^ {/ szöveg hozzáfűzése, mert az összes 3 PNS-hívások is információk tooview [self.sendResults setText:[self.sendResults.text stringByAppendingString:status]];            });            NSLog(status);        }

                if (data != NULL)
                {
                    xmlParser = [[NSXMLParser alloc] initWithData:data];
                    [xmlParser setDelegate:self];
                    [xmlParser parse];
                }
            }];    [dataTask folytatása]; }


1. A hello hello műveletének frissítése **értesítés küldése** toouse hello ASP.NET-háttérrendszerből gombra, és a kapcsoló által engedélyezett PNS tooany küldése.

        - (IBAction)SendNotificationMessage:(id)sender
        {
            //[self SendNotificationRESTAPI];
            [self SendToEnabledPlatforms];
        }


        -(void)SendToEnabledPlatforms
        {
            NSString* json = [NSString stringWithFormat:@"\"%@\"",self.notificationMessage.text];

            [self.sendResults setText:@""];

            if ([self.WNSSwitch isOn])
                [self SendNotificationASPNETBackend:@"wns" UsernameTag:self.RecipientField.text Message:json];

            if ([self.GCMSwitch isOn])
                [self SendNotificationASPNETBackend:@"gcm" UsernameTag:self.RecipientField.text Message:json];

            if ([self.APNSSwitch isOn])
                [self SendNotificationASPNETBackend:@"apns" UsernameTag:self.RecipientField.text Message:json];
        }



1. A függvény **ViewDidLoad**adja hozzá a következő tooinstantiate hello RegisterClient példány hello és állítsa be a szövegmezők hello delegált.
   
       self.UsernameField.delegate = self;
       self.PasswordField.delegate = self;
       self.RecipientField.delegate = self;
       self.registerClient = [[RegisterClient alloc] initWithEndpoint:BACKEND_ENDPOINT];
2. Most a **AppDelegate.m**, távolítsa el az összes hello tartalom hello metódus **alkalmazás: didRegisterForPushNotificationWithDeviceToken:** és cserélje le, hogy a következő toomake hello, hello nézet a tartományvezérlő hello APNs lekért legutóbbi eszköz jogkivonatát tartalmazza:
   
       // Add import toohello top of hello file
       #import "ViewController.h"
   
       - (void)application:(UIApplication *)application
                   didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken
       {
           ViewController* rvc = (ViewController*) self.window.rootViewController;
           rvc.deviceToken = deviceToken;
       }
3. Végül a **AppDelegate.m**, győződjön meg arról, hogy a következő metódus hello:
   
       - (void)application:(UIApplication *)application didReceiveRemoteNotification: (NSDictionary *)userInfo {
           NSLog(@"%@", userInfo);
           [self MessageBox:@"Notification" message:[[userInfo objectForKey:@"aps"] valueForKey:@"alert"]];
       }

## <a name="test-hello-application"></a>Teszt hello alkalmazás
1. Az XCode-ban hello alkalmazás futtatása egy fizikai iOS-eszközön (leküldéses értesítések hello szimulátor fog működni).
2. A hello iOS-alkalmazás felhasználói felületén írja be a felhasználónevet és jelszót. Bármilyen karakterlánc is lehetnek, de azok hello lehet ugyanaz a karakterlánc értéke. Kattintson a **bejelentkezés**.
   
    ![][2]
3. Meg kell jelennie egy előugró ablak regisztráció sikeres ad tájékoztatást. Kattintson az **OK** gombra.
   
    ![][3]
4. A hello **címzett felhasználónév címke* szöveg mezőbe írja be a hello felhasználói név címke használt hello regisztrálása egy másik eszközről.
5. Adjon meg egy értesítési üzenetet, és kattintson a **értesítés küldése**.  Csak egy regisztrációs hello címzett felhasználói név címkével rendelkező hello eszköz hello értesítési üzenetet küld.  Csak továbbítás toothose felhasználók.
   
    ![][4]

[1]: ./media/notification-hubs-aspnet-backend-ios-notify-users/notification-hubs-ios-notify-users-interface.png
[2]: ./media/notification-hubs-aspnet-backend-ios-notify-users/notification-hubs-ios-notify-users-enter-user-pwd.png
[3]: ./media/notification-hubs-aspnet-backend-ios-notify-users/notification-hubs-ios-notify-users-registered.png
[4]: ./media/notification-hubs-aspnet-backend-ios-notify-users/notification-hubs-ios-notify-users-enter-msg.png
