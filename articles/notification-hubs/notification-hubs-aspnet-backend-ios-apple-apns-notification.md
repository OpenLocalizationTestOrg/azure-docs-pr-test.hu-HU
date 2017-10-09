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
# <a name="azure-notification-hubs-notify-users-for-ios-with-net-backend"></a><span data-ttu-id="b3605-104">Azure Notification Hubs – felhasználók értesítése iOS rendszerhez .NET-háttérrendszerrel</span><span class="sxs-lookup"><span data-stu-id="b3605-104">Azure Notification Hubs Notify Users for iOS with .NET backend</span></span>
[!INCLUDE [notification-hubs-selector-aspnet-backend-notify-users](../../includes/notification-hubs-selector-aspnet-backend-notify-users.md)]

## <a name="overview"></a><span data-ttu-id="b3605-105">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="b3605-105">Overview</span></span>
<span data-ttu-id="b3605-106">Leküldéses értesítési támogatás az Azure-ban lehetővé teszi tooaccess egy könnyen kezelhető, multiplatform és kibővített leküldéses infrastruktúrában, ami jelentősen egyszerűbb a leküldéses értesítések a mobile fogyasztói, valamint a vállalati alkalmazások hello végrehajtása platformok.</span><span class="sxs-lookup"><span data-stu-id="b3605-106">Push notification support in Azure enables you tooaccess an easy-to-use, multiplatform, and scaled-out push infrastructure, which greatly simplifies hello implementation of push notifications for both consumer and enterprise applications for mobile platforms.</span></span> <span data-ttu-id="b3605-107">Ez az oktatóanyag toouse Azure Notification Hubs toosend a leküldéses értesítések tooa adott alkalmazás felhasználói az adott eszközön.</span><span class="sxs-lookup"><span data-stu-id="b3605-107">This tutorial shows you how toouse Azure Notification Hubs toosend push notifications tooa specific app user on a specific device.</span></span> <span data-ttu-id="b3605-108">ASP.NET WebAPI háttérrendszerből használt tooauthenticate ügyfelek és toogenerate értesítéseket, ahogy hello útmutatást a témakör az [az alkalmazás háttérrendszeréből regisztrálása](notification-hubs-push-notification-registration-management.md#registration-management-from-a-backend).</span><span class="sxs-lookup"><span data-stu-id="b3605-108">An ASP.NET WebAPI backend is used tooauthenticate clients and toogenerate notifications, as shown in hello guidance topic [Registering from your app backend](notification-hubs-push-notification-registration-management.md#registration-management-from-a-backend).</span></span>

> [!NOTE]
> <span data-ttu-id="b3605-109">Ez az oktatóanyag feltételezi, hogy létrehozta és leírtak szerint konfigurálta az értesítési központ [Ismerkedés a Notification Hubs (iOS)](notification-hubs-ios-apple-push-notification-apns-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="b3605-109">This tutorial assumes that you have created and configured your notification hub as described in [Getting Started with Notification Hubs (iOS)](notification-hubs-ios-apple-push-notification-apns-get-started.md).</span></span> <span data-ttu-id="b3605-110">Ez az oktatóanyag egyben hello előfeltétel toohello [(iOS) biztonságos leküldéses](notification-hubs-aspnet-backend-ios-push-apple-apns-secure-notification.md) oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="b3605-110">This tutorial is also hello prerequisite toohello [Secure Push (iOS)](notification-hubs-aspnet-backend-ios-push-apple-apns-secure-notification.md) tutorial.</span></span>
> <span data-ttu-id="b3605-111">Ha a háttérkiszolgáló szolgáltatásként toouse Mobile Apps, lásd: hello [Mobile Apps első lépések leküldéses](../app-service-mobile/app-service-mobile-ios-get-started-push.md).</span><span class="sxs-lookup"><span data-stu-id="b3605-111">If you want toouse Mobile Apps as your backend service, see hello [Mobile Apps Get Started with Push](../app-service-mobile/app-service-mobile-ios-get-started-push.md).</span></span>
> 
> 

[!INCLUDE [notification-hubs-aspnet-backend-notifyusers](../../includes/notification-hubs-aspnet-backend-notifyusers.md)]

## <a name="modify-your-ios-app"></a><span data-ttu-id="b3605-112">Az iOS-alkalmazás módosítása</span><span class="sxs-lookup"><span data-stu-id="b3605-112">Modify your iOS app</span></span>
1. <span data-ttu-id="b3605-113">Nyissa meg hello egylapos alkalmazás megtekintése hello létrehozott [Ismerkedés a Notification Hubs (iOS)](notification-hubs-ios-apple-push-notification-apns-get-started.md) oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="b3605-113">Open hello Single Page view app you created in hello [Getting Started with Notification Hubs (iOS)](notification-hubs-ios-apple-push-notification-apns-get-started.md) tutorial.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="b3605-114">Ebben a szakaszban feltételezzük, hogy a projekt egy üres szervezetnév van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="b3605-114">In this section we assume that your project is configured with an empty organization name.</span></span> <span data-ttu-id="b3605-115">Ha nem, akkor tooprepend a szervezet neve tooall osztály nevét.</span><span class="sxs-lookup"><span data-stu-id="b3605-115">If not, you will need tooprepend your organization name tooall class names.</span></span>
   > 
   > 
2. <span data-ttu-id="b3605-116">Adja hozzá a Main.storyboard hello összetevők hello objektum könyvtárból alatt hello képernyőfelvételen látható módon.</span><span class="sxs-lookup"><span data-stu-id="b3605-116">In your Main.storyboard add hello components shown in hello screenshot below from hello object library.</span></span>
   
    ![][1]
   
   * <span data-ttu-id="b3605-117">**Felhasználónév**: a helyőrző szöveg A UITextField *adjon meg felhasználónevet*, azonnal hello alatt eredmények címke és a bal oldali korlátozott toohello küldését és jobb margók és hello alatt küldése az eredmények címke.</span><span class="sxs-lookup"><span data-stu-id="b3605-117">**Username**: A UITextField with placeholder text, *Enter Username*, immediately beneath hello send results label and constrained toohello left and right margins and beneath hello send results label.</span></span>
   * <span data-ttu-id="b3605-118">**Jelszó**: a helyőrző szöveg A UITextField *jelszó megadása*, azonnal kijelölése hello felhasználónév szövegmezőt és korlátozott toohello bal és jobb margók és hello felhasználónév szövegmező alatt.</span><span class="sxs-lookup"><span data-stu-id="b3605-118">**Password**: A UITextField with placeholder text, *Enter Password*, immediately beneath hello username text field and constrained toohello left and right margins and beneath hello username text field.</span></span> <span data-ttu-id="b3605-119">Ellenőrizze a hello **szövegbeviteli biztonságos** hello attribútum megtekintő lehetőséget a *vissza kulcs*.</span><span class="sxs-lookup"><span data-stu-id="b3605-119">Check hello **Secure Text Entry** option in hello Attribute Inspector, under *Return Key*.</span></span>
   * <span data-ttu-id="b3605-120">**Jelentkezzen be**: A UIButton feliratú azonnal hello jelszó szövegmező alatt, és törölje a hello **engedélyezve** hello attribútumok megtekintő lehetőséget a *vezérlő – tartalom*</span><span class="sxs-lookup"><span data-stu-id="b3605-120">**Log in**: A UIButton labeled immediately beneath hello password text field and uncheck hello **Enabled** option in hello Attributes Inspector, under *Control-Content*</span></span>
   * <span data-ttu-id="b3605-121">**WNS**: címke és kapcsoló tooenable küldése hello értesítési Windows értesítési szolgáltatásával, ha a telepítő hello központ le van.</span><span class="sxs-lookup"><span data-stu-id="b3605-121">**WNS**: Label and switch tooenable sending hello notification Windows Notification Service if it has been setup on hello hub.</span></span> <span data-ttu-id="b3605-122">Lásd: hello [Windows bevezetés](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="b3605-122">See hello [Windows Getting Started](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) tutorial.</span></span>
   * <span data-ttu-id="b3605-123">**GCM**: címke és kapcsoló tooenable küldése hello értesítési tooGoogle Cloud Messaging, ha a telepítő hello központ le van.</span><span class="sxs-lookup"><span data-stu-id="b3605-123">**GCM**: Label and switch tooenable sending hello notification tooGoogle Cloud Messaging if it has been setup on hello hub.</span></span> <span data-ttu-id="b3605-124">Lásd: [Android bevezetés](notification-hubs-android-push-notification-google-gcm-get-started.md) oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="b3605-124">See [Android Getting Started](notification-hubs-android-push-notification-google-gcm-get-started.md) tutorial.</span></span>
   * <span data-ttu-id="b3605-125">**APNS**: címke és kapcsoló tooenable küldése hello Apple Platform Notification szolgáltatáshoz értesítést toohello.</span><span class="sxs-lookup"><span data-stu-id="b3605-125">**APNS**: Label and switch tooenable sending hello notification toohello Apple Platform Notification Service.</span></span>
   * <span data-ttu-id="b3605-126">**Recipent felhasználónév**: a helyőrző szöveg A UITextField *címzett felhasználónév címke*, azonnal alatt hello GCM címke és korlátozott toohello bal és jobb margók és hello GCM alatt címkézését.</span><span class="sxs-lookup"><span data-stu-id="b3605-126">**Recipent Username**:A UITextField with placeholder text, *Recipient username tag*, immediately beneath hello GCM label and constrained toohello left and right margins and beneath hello GCM label.</span></span>

    <span data-ttu-id="b3605-127">Néhány összetevőt hozzáadott hello [Ismerkedés a Notification Hubs (iOS)](notification-hubs-ios-apple-push-notification-apns-get-started.md) oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="b3605-127">Some components were added in hello [Getting Started with Notification Hubs (iOS)](notification-hubs-ios-apple-push-notification-apns-get-started.md) tutorial.</span></span>

1. <span data-ttu-id="b3605-128">**CTRL** az egérrel húzzon át hello nézet tooViewController.h hello összetevőket, és adja hozzá ezek új értékesítési lehetőségek.</span><span class="sxs-lookup"><span data-stu-id="b3605-128">**Ctrl** drag from hello components in hello view tooViewController.h and add these new outlets.</span></span>
   
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
2. <span data-ttu-id="b3605-129">A ViewController.h, adja hozzá a hello következő `#define` alatt az importálási utasítást.</span><span class="sxs-lookup"><span data-stu-id="b3605-129">In ViewController.h, add hello following `#define` just below your import statements.</span></span> <span data-ttu-id="b3605-130">Helyettesítő hello *< írja be a háttérkiszolgáló végpont\>*  helyőrzőt hello használt toodeploy a háttéralkalmazás hello előző szakaszban cél URL-címe.</span><span class="sxs-lookup"><span data-stu-id="b3605-130">Substitute hello *<Enter Your Backend Endpoint\>* placeholder with hello Destination URL you used toodeploy your app backend in hello previous section.</span></span> <span data-ttu-id="b3605-131">Például *http://you_backend.azurewebsites.net*.</span><span class="sxs-lookup"><span data-stu-id="b3605-131">For example, *http://you_backend.azurewebsites.net*.</span></span>
   
        #define BACKEND_ENDPOINT @"<Enter Your Backend Endpoint>"
3. <span data-ttu-id="b3605-132">A projekt, hozzon létre egy új **Cocoa Touch osztály** nevű **RegisterClient** az ASP.NET-háttér létrehozott hello toointerface.</span><span class="sxs-lookup"><span data-stu-id="b3605-132">In your project, create a new **Cocoa Touch class** named **RegisterClient** toointerface with hello ASP.NET back-end you created.</span></span> <span data-ttu-id="b3605-133">Hozzon létre hello osztályt való öröklődés `NSObject`.</span><span class="sxs-lookup"><span data-stu-id="b3605-133">Create hello class inheriting from `NSObject`.</span></span> <span data-ttu-id="b3605-134">Majd adja hozzá a következő kódot a hello RegisterClient.h hello.</span><span class="sxs-lookup"><span data-stu-id="b3605-134">Then add hello following code in hello RegisterClient.h.</span></span>
   
        @interface RegisterClient : NSObject
   
        @property (strong, nonatomic) NSString* authenticationHeader;
   
        -(void) registerWithDeviceToken:(NSData*)token tags:(NSSet*)tags
            andCompletion:(void(^)(NSError*))completion;
   
        -(instancetype) initWithEndpoint:(NSString*)Endpoint;
   
        @end
4. <span data-ttu-id="b3605-135">Hello RegisterClient.m frissítse hello `@interface` szakasz:</span><span class="sxs-lookup"><span data-stu-id="b3605-135">In hello RegisterClient.m update hello `@interface` section:</span></span>
   
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
5. <span data-ttu-id="b3605-136">Cserélje le a hello `@implementation` hello RegisterClient.m a következő kód hello szakaszában.</span><span class="sxs-lookup"><span data-stu-id="b3605-136">Replace hello `@implementation` section in hello RegisterClient.m with hello following code.</span></span>

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

    <span data-ttu-id="b3605-137">a fenti hello kódot megvalósítja hello útmutatást a cikkben ismertetett hello logika [az alkalmazás háttérrendszeréből regisztrálása](notification-hubs-push-notification-registration-management.md#registration-management-from-a-backend) NSURLSession használatával tooperform REST meghívja tooyour háttéralkalmazás és NSUserDefaults toolocally tároló hello hello értesítési központ által visszaadott registrationId.</span><span class="sxs-lookup"><span data-stu-id="b3605-137">hello code above implements hello logic explained in hello guidance article [Registering from your app backend](notification-hubs-push-notification-registration-management.md#registration-management-from-a-backend) using NSURLSession tooperform REST calls tooyour app backend, and NSUserDefaults toolocally store hello registrationId returned by hello notification hub.</span></span>

    <span data-ttu-id="b3605-138">Vegye figyelembe, hogy ez az osztály van szükség a tulajdonsága **authorizationHeader** toobe rendelés toowork megfelelően beállítva.</span><span class="sxs-lookup"><span data-stu-id="b3605-138">Note that this class requires its property **authorizationHeader** toobe set in order toowork properly.</span></span> <span data-ttu-id="b3605-139">A tulajdonság értéke által hello **ViewController** osztály után hello jelentkezzen be.</span><span class="sxs-lookup"><span data-stu-id="b3605-139">This property is set by hello **ViewController** class after hello log in.</span></span>

1. <span data-ttu-id="b3605-140">A ViewController.h, adjon hozzá egy `#import` RegisterClient.h utasítást.</span><span class="sxs-lookup"><span data-stu-id="b3605-140">In ViewController.h, add a `#import` statement for RegisterClient.h.</span></span> <span data-ttu-id="b3605-141">Majd adjon hozzá egy; hello eszköz jogkivonat, és hivatkozzon tooa `RegisterClient` példánya a hello `@interface` szakasz:</span><span class="sxs-lookup"><span data-stu-id="b3605-141">Then add a declaration for hello device token and reference tooa `RegisterClient` instance in hello `@interface` section:</span></span>
   
        #import "RegisterClient.h"
   
        @property (strong, nonatomic) NSData* deviceToken;
        @property (strong, nonatomic) RegisterClient* registerClient;
2. <span data-ttu-id="b3605-142">ViewController.m, adja hozzá a titkos metódus deklarációjában a hello `@interface` szakasz:</span><span class="sxs-lookup"><span data-stu-id="b3605-142">In ViewController.m, add a private method declaration in hello `@interface` section:</span></span>
   
        @interface ViewController () <UITextFieldDelegate, NSURLConnectionDataDelegate, NSXMLParserDelegate>
   
        // create hello Authorization header tooperform Basic authentication with your app back-end
        -(void) createAndSetAuthenticationHeaderWithUsername:(NSString*)username
                        AndPassword:(NSString*)password;
   
        @end

> [!NOTE]
> <span data-ttu-id="b3605-143">hello alábbi részlet nem egy biztonságos hitelesítési séma, helyettesítse be hello hello végrehajtásának **createAndSetAuthenticationHeaderWithUsername:AndPassword:** , az adott hitelesítési mechanizmus egy hello register ügyfél osztály, például az OAuth, az Active Directory által használt hitelesítési jogkivonat toobe létrehozó.</span><span class="sxs-lookup"><span data-stu-id="b3605-143">hello following snippet is not a secure authentication scheme, you should substitute hello implementation of hello **createAndSetAuthenticationHeaderWithUsername:AndPassword:** with your specific authentication mechanism that generates an authentication token toobe consumed by hello register client class, e.g. OAuth, Active Directory.</span></span>
> 
> 

1. <span data-ttu-id="b3605-144">Ezt a hello `@implementation` ViewController.m szakasza adja hozzá a következő kód, amely hello megvalósítási beállítás hello eszköz token és hitelesítési fejléc hozzáadása hello.</span><span class="sxs-lookup"><span data-stu-id="b3605-144">Then in hello `@implementation` section of ViewController.m add hello following code which adds hello implementation for setting hello device token and authentication header.</span></span>
   
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
   
    <span data-ttu-id="b3605-145">Vegye figyelembe, hogyan beállítás hello eszköz jogkivonatát lehetővé teszi, hogy hello bejelentkezés gombra.</span><span class="sxs-lookup"><span data-stu-id="b3605-145">Note how setting hello device token enables hello log in button.</span></span> <span data-ttu-id="b3605-146">Ennek az az oka hello bejelentkezési művelet részeként a hello nézetvezérlő regisztrálja a leküldéses értesítések az hello háttéralkalmazás.</span><span class="sxs-lookup"><span data-stu-id="b3605-146">This is becasue as a part of hello login action, hello view controller registers for push notifications with hello app backend.</span></span> <span data-ttu-id="b3605-147">Emiatt nem szeretnénk bejelentkezési művelet toobe elérhető keretein belül hello eszköz jogkivonatát megfelelően be van állítva.</span><span class="sxs-lookup"><span data-stu-id="b3605-147">Hence, we do not want Log In action toobe accessible till hello device token has been properly set up.</span></span> <span data-ttu-id="b3605-148">Hello leküldéses regisztráció a bejelentkezéshez hello is absztrakcióhoz, mindaddig, amíg hello volt ez utóbbi hello előtt történik.</span><span class="sxs-lookup"><span data-stu-id="b3605-148">You can decouple hello log in from hello push registration as long as hello former happens before hello latter.</span></span>
2. <span data-ttu-id="b3605-149">A ViewController.m, használja az alábbi kódtöredékek tooimplement hello tartozó műveleti módszer a hello a **bejelentkezés** gombra, és egy metódus toosend hello értesítési üzenet használatával hello ASP.NET-háttérrendszerből.</span><span class="sxs-lookup"><span data-stu-id="b3605-149">In ViewController.m, use hello following snippets tooimplement hello action method for your **Log In** button and a method toosend hello notification message using hello ASP.NET backend.</span></span>
   
       - <span data-ttu-id="b3605-150">(IBAction) LogInAction: (id) küldő {/ / hitelesítési fejléc létrehozásáról és a nyilvántartás ügyfél NSString * felhasználónév = saját maga. UsernameField.text;   NSString * jelszó = saját maga. PasswordField.text;</span><span class="sxs-lookup"><span data-stu-id="b3605-150">(IBAction)LogInAction:(id)sender {   // create authentication header and set it in register client   NSString* username = self.UsernameField.text;   NSString* password = self.PasswordField.text;</span></span>
   
           <span data-ttu-id="b3605-151">[az önkiszolgáló createAndSetAuthenticationHeaderWithUsername:username AndPassword:password];</span><span class="sxs-lookup"><span data-stu-id="b3605-151">[self createAndSetAuthenticationHeaderWithUsername:username AndPassword:password];</span></span>
   
           <span data-ttu-id="b3605-152">__weak ViewController * selfie = önkiszolgáló;   [self.registerClient registerWithDeviceToken:self.deviceToken címkék: üres andCompletion:^(NSError* error) {Ha (! hiba) {dispatch_async(dispatch_get_main_queue(), ^ {selfie. SendNotificationButton.enabled = YES;               [self MessageBox:@"Success" message:@"Registered sikeresen!"];});}}];}</span><span class="sxs-lookup"><span data-stu-id="b3605-152">__weak ViewController* selfie = self;   [self.registerClient registerWithDeviceToken:self.deviceToken tags:nil       andCompletion:^(NSError* error) {       if (!error) {           dispatch_async(dispatch_get_main_queue(),           ^{               selfie.SendNotificationButton.enabled = YES;               [self MessageBox:@"Success" message:@"Registered successfully!"];           });       }   }]; }</span></span>

        <span data-ttu-id="b3605-153">-SendNotificationASPNETBackend (a "void"): (NSString*) pns UsernameTag: (NSString*) usernameTag üzenetet: (NSString*) üzenet {NSURLSession* munkamenet = [NSURLSession sessionWithConfiguration: [ NSURLSessionConfiguration defaultSessionConfiguration] delegált: üres delegateQueue:nil];</span><span class="sxs-lookup"><span data-stu-id="b3605-153">- (void)SendNotificationASPNETBackend:(NSString*)pns UsernameTag:(NSString*)usernameTag            Message:(NSString*)message {    NSURLSession* session = [NSURLSession        sessionWithConfiguration:[NSURLSessionConfiguration defaultSessionConfiguration] delegate:nil        delegateQueue:nil];</span></span>

            <span data-ttu-id="b3605-154">A hello többi URL-cím toohello ASP.NET háttér által igényelt NSURL * requestURL paraméterként hello pns-sel és a username címke átadni = [által igényelt NSURL URLWithString: [NSString stringWithFormat:@"%@/api/notifications? pns = % @& to_tag = % @", BACKEND_ENDPOINT, pns, usernameTag]];</span><span class="sxs-lookup"><span data-stu-id="b3605-154">// Pass hello pns and username tag as parameters with hello REST URL toohello ASP.NET backend    NSURL* requestURL = [NSURL URLWithString:[NSString        stringWithFormat:@"%@/api/notifications?pns=%@&to_tag=%@", BACKEND_ENDPOINT, pns,        usernameTag]];</span></span>

            <span data-ttu-id="b3605-155">NSMutableURLRequest * kérelem = [NSMutableURLRequest requestWithURL:requestURL];    [lekérő setHTTPMethod:@"POST"];</span><span class="sxs-lookup"><span data-stu-id="b3605-155">NSMutableURLRequest* request = [NSMutableURLRequest requestWithURL:requestURL];    [request setHTTPMethod:@"POST"];</span></span>

            <span data-ttu-id="b3605-156">Hello utánzatait authenticationheader lehívása hello register ügyfél NSString * authorizationHeaderValue = [NSString stringWithFormat:@"Basic % @", self.registerClient.authenticationHeader];    [lekérő setValue:authorizationHeaderValue forHTTPHeaderField:@"Authorization"];</span><span class="sxs-lookup"><span data-stu-id="b3605-156">// Get hello mock authenticationheader from hello register client    NSString* authorizationHeaderValue = [NSString stringWithFormat:@"Basic %@",        self.registerClient.authenticationHeader];    [request setValue:authorizationHeaderValue forHTTPHeaderField:@"Authorization"];</span></span>

            <span data-ttu-id="b3605-157">Adja hozzá a hello értesítési üzenet törzse [setValue:@"application/json;charset=utf-8 kérelem" forHTTPHeaderField:@"Content-Type"];    [setHTTPBody kérése: [üzenet dataUsingEncoding:NSUTF8StringEncoding]];</span><span class="sxs-lookup"><span data-stu-id="b3605-157">//Add hello notification message body    [request setValue:@"application/json;charset=utf-8" forHTTPHeaderField:@"Content-Type"];    [request setHTTPBody:[message dataUsingEncoding:NSUTF8StringEncoding]];</span></span>

            <span data-ttu-id="b3605-158">Hello ASP.NET háttér NSURLSessionDataTask * dataTask hello send notification REST API-t végrehajtása [munkamenet dataTaskWithRequest:request completionHandler:^(NSData *data, NSURLResponse *response, NSError  *= Hiba történt) {NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) választ;        Ha (hiba || httpResponse.statusCode! = 200) {NSString* állapot = [NSString stringWithFormat:@"Error állapota a(z) % @: % d\nError: %@\n", pns, httpResponse.statusCode, hiba];            dispatch_async(dispatch_get_main_queue(), ^ {/ szöveg hozzáfűzése, mert az összes 3 PNS-hívások is információk tooview [self.sendResults setText:[self.sendResults.text stringByAppendingString:status]];            });            NSLog(status);        }</span><span class="sxs-lookup"><span data-stu-id="b3605-158">// Execute hello send notification REST API on hello ASP.NET Backend    NSURLSessionDataTask* dataTask = [session dataTaskWithRequest:request        completionHandler:^(NSData *data, NSURLResponse *response, NSError *error)    {        NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;        if (error || httpResponse.statusCode != 200)        {            NSString* status = [NSString stringWithFormat:@"Error Status for %@: %d\nError: %@\n",                                pns, httpResponse.statusCode, error];            dispatch_async(dispatch_get_main_queue(),            ^{                // Append text because all 3 PNS calls may also have information tooview                [self.sendResults setText:[self.sendResults.text stringByAppendingString:status]];            });            NSLog(status);        }</span></span>

                if (data != NULL)
                {
                    xmlParser = [[NSXMLParser alloc] initWithData:data];
                    [xmlParser setDelegate:self];
                    [xmlParser parse];
                }
            <span data-ttu-id="b3605-159">}];    [dataTask folytatása]; }</span><span class="sxs-lookup"><span data-stu-id="b3605-159">}];    [dataTask resume]; }</span></span>


1. <span data-ttu-id="b3605-160">A hello hello műveletének frissítése **értesítés küldése** toouse hello ASP.NET-háttérrendszerből gombra, és a kapcsoló által engedélyezett PNS tooany küldése.</span><span class="sxs-lookup"><span data-stu-id="b3605-160">Update hello action for hello **Send Notification** button toouse hello ASP.NET backend and send tooany PNS enabled by a switch.</span></span>

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



1. <span data-ttu-id="b3605-161">A függvény **ViewDidLoad**adja hozzá a következő tooinstantiate hello RegisterClient példány hello és állítsa be a szövegmezők hello delegált.</span><span class="sxs-lookup"><span data-stu-id="b3605-161">In function **ViewDidLoad**, add hello following tooinstantiate hello RegisterClient instance and set hello delegate for your text fields.</span></span>
   
       self.UsernameField.delegate = self;
       self.PasswordField.delegate = self;
       self.RecipientField.delegate = self;
       self.registerClient = [[RegisterClient alloc] initWithEndpoint:BACKEND_ENDPOINT];
2. <span data-ttu-id="b3605-162">Most a **AppDelegate.m**, távolítsa el az összes hello tartalom hello metódus **alkalmazás: didRegisterForPushNotificationWithDeviceToken:** és cserélje le, hogy a következő toomake hello, hello nézet a tartományvezérlő hello APNs lekért legutóbbi eszköz jogkivonatát tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="b3605-162">Now in **AppDelegate.m**, remove all hello content of hello method **application:didRegisterForPushNotificationWithDeviceToken:** and replace it with hello following toomake sure that hello view controller contains hello latest device token retrieved from APNs:</span></span>
   
       // Add import toohello top of hello file
       #import "ViewController.h"
   
       - (void)application:(UIApplication *)application
                   didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken
       {
           ViewController* rvc = (ViewController*) self.window.rootViewController;
           rvc.deviceToken = deviceToken;
       }
3. <span data-ttu-id="b3605-163">Végül a **AppDelegate.m**, győződjön meg arról, hogy a következő metódus hello:</span><span class="sxs-lookup"><span data-stu-id="b3605-163">Finally in **AppDelegate.m**, make sure you have hello following method:</span></span>
   
       - (void)application:(UIApplication *)application didReceiveRemoteNotification: (NSDictionary *)userInfo {
           NSLog(@"%@", userInfo);
           [self MessageBox:@"Notification" message:[[userInfo objectForKey:@"aps"] valueForKey:@"alert"]];
       }

## <a name="test-hello-application"></a><span data-ttu-id="b3605-164">Teszt hello alkalmazás</span><span class="sxs-lookup"><span data-stu-id="b3605-164">Test hello Application</span></span>
1. <span data-ttu-id="b3605-165">Az XCode-ban hello alkalmazás futtatása egy fizikai iOS-eszközön (leküldéses értesítések hello szimulátor fog működni).</span><span class="sxs-lookup"><span data-stu-id="b3605-165">In XCode, run hello app on a physical iOS device (push notifications will not work in hello simulator).</span></span>
2. <span data-ttu-id="b3605-166">A hello iOS-alkalmazás felhasználói felületén írja be a felhasználónevet és jelszót.</span><span class="sxs-lookup"><span data-stu-id="b3605-166">In hello iOS app UI, enter a username and password.</span></span> <span data-ttu-id="b3605-167">Bármilyen karakterlánc is lehetnek, de azok hello lehet ugyanaz a karakterlánc értéke.</span><span class="sxs-lookup"><span data-stu-id="b3605-167">These can be any string, but they must both be hello same string value.</span></span> <span data-ttu-id="b3605-168">Kattintson a **bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="b3605-168">Then click **Log In**.</span></span>
   
    ![][2]
3. <span data-ttu-id="b3605-169">Meg kell jelennie egy előugró ablak regisztráció sikeres ad tájékoztatást.</span><span class="sxs-lookup"><span data-stu-id="b3605-169">You should see a pop-up informing you of registration success.</span></span> <span data-ttu-id="b3605-170">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="b3605-170">Click **OK**.</span></span>
   
    ![][3]
4. <span data-ttu-id="b3605-171">A hello **címzett felhasználónév címke* szöveg mezőbe írja be a hello felhasználói név címke használt hello regisztrálása egy másik eszközről.</span><span class="sxs-lookup"><span data-stu-id="b3605-171">In hello **Recipient username tag* text field, enter hello user name tag used with hello registration from another device.</span></span>
5. <span data-ttu-id="b3605-172">Adjon meg egy értesítési üzenetet, és kattintson a **értesítés küldése**.</span><span class="sxs-lookup"><span data-stu-id="b3605-172">Enter a notification message and click **Send Notification**.</span></span>  <span data-ttu-id="b3605-173">Csak egy regisztrációs hello címzett felhasználói név címkével rendelkező hello eszköz hello értesítési üzenetet küld.</span><span class="sxs-lookup"><span data-stu-id="b3605-173">Only hello devices that have a registration with hello recipient user name tag receive hello notification message.</span></span>  <span data-ttu-id="b3605-174">Csak továbbítás toothose felhasználók.</span><span class="sxs-lookup"><span data-stu-id="b3605-174">It is only sent toothose users.</span></span>
   
    ![][4]

[1]: ./media/notification-hubs-aspnet-backend-ios-notify-users/notification-hubs-ios-notify-users-interface.png
[2]: ./media/notification-hubs-aspnet-backend-ios-notify-users/notification-hubs-ios-notify-users-enter-user-pwd.png
[3]: ./media/notification-hubs-aspnet-backend-ios-notify-users/notification-hubs-ios-notify-users-registered.png
[4]: ./media/notification-hubs-aspnet-backend-ios-notify-users/notification-hubs-ios-notify-users-enter-msg.png
